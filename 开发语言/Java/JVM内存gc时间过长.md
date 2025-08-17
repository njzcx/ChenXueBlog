### 现象

web应用启动后正常工作，但一段时间后，整体响应变慢，请求时间达到十多秒

### 排查过程

#### 1. 应用日志

经查看应用日志，并用arthas trace方法耗时，发现从接收到响应请求花费时间并不多，排出是应用程序处理的问题

#### 2. 主机状况

查看主机CPU和应用进程。发现CPU飙升，达到核数上限，且CPU均被应用进程占用，该进程占用内存也达到Xmx值4G，至此初步怀疑是应用内存溢出

#### 3. 查看GC情况

应用启动参数增加 -verbose:gc -Xloggc:/home/admin/logs/gc.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps。

获取应用gc.log上传到[https://gceasy.io](https://gceasy.io/) 或集团环境[Grace](https://grace-parser.alibaba-inc.com/)(**ATP**)，分析gc.log。如下图所示，其中gc暂停时间最大为16秒，且在27日4点后有极高频率的full gc，老年代空间被完全耗尽。至此已明确CPU飙升是由于GC引起，而频繁的Full GC是由于内存泄漏导致老年代空间耗尽。

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发语言/Java/images/JVM内存gc时间过长/1720081856059-73055ee4-3db9-48f6-88e1-b0fb47dc3d7a.png)

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发语言/Java/images/JVM内存gc时间过长/1720082068852-abbd586e-03fe-445d-b1b3-efb1e847694e.png)

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发语言/Java/images/JVM内存gc时间过长/1720082136057-86c514a2-3080-4b58-a5d2-943d5fd67b4b.png)

#### 4. dump分析

在容器内运行jmap -dump:format=b,file=dumpfile <进程pid>，将生成dump文件。

导入到MAT/VisualVM中分析定位泄漏和大对象，建议使用MAT分析。

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发语言/Java/images/JVM内存gc时间过长/1720085288774-8fb0903d-740f-4c4a-8a70-4df9e771c1a4.png)

1. **定位泄漏对象（方法一）**：点击上图中Histogram，可显示出每个类产生的实例数量，以及所占用的内存大小；

Shallow Heap 和 Retained Heap分别表示对象自身不包含引用的大小和对象自身并包含引用的大小。默认的大小单位是 Bytes，可以在 Window - Preferences 菜单中设置单位，图中设置的是KB。

根据Shallow Heap 和 Retained Heap找出占用最大的对象（下图）

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发语言/Java/images/JVM内存gc时间过长/1720084926642-47d55f86-24c5-4479-93be-0c78a90906ca.png)

第2行线程池数量达到4万多，且内存占用1.6G。这时已经大概该对象占用了，根据项目实际情况分析大概率能定位到原因。本问题经判断是由于频繁new HttpClient且未释放，而每个HttpClient内部会创建线程池，导致线程池越来越多。

2. **定位泄漏对象（方法二）**：点击MAT overview中 Leak Suspects 后，MAT会自动分析生成一份报告，将展示存在的泄漏问题以及大对象，如下图所示，存在2个问题，也与上一步结果相呼应。

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发语言/Java/images/JVM内存gc时间过长/1720084696806-f1bfc812-14e5-449d-aabc-0d776dcd3184.png)

### 处理方式

1. 修改代码中可能导致对象创建的位置，并测试是否复现。本问题中，将每次new HttpClient，使用完后close释放，得到解决。

### 排查过程中的问题

#### 1. JvisualVM无法分析大文件

dump文件3G左右，用 visualvm 查看时提示 堆查看器使用的内存不足。解决办法是将visualVM启动参数Xmx改大

修改JAVA_HOME/lib/visualvm/etc/visualvm.conf文件中 visualvm_default_options="-J-client -J-Xms24 -J-Xmx256m"，把256改为2048，然后重启jvisualVM即可。

如果不知道 JAVA_HOME 目录， 可以 通过 echo $JAVA_HOME 查看

#### 2. MAT安装和使用

1. 通过官网下载MAT：[Eclipse Memory Analyzer Open Source Project | The Eclipse Foundation](https://eclipse.dev/mat/downloads.php)
2. 下载MAT对应的JDK，本文MAT需要使用JDK17：[Java Archive Downloads - Java SE 17](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
3. 修改MAT启动参数，指定JDK17目录

1. 应用里右击mat选择显示包内容

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发语言/Java/images/JVM内存gc时间过长/1720088263685-528695c0-be01-404c-913c-89aa9a6cde88.png)![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发语言/Java/images/JVM内存gc时间过长/1720088371757-fcbaee2a-3311-4de0-9af5-11cf490053be.png)

1. 编辑Info.plist，找到 `<array>` 添加 jdk 路径

```xml
<array>
  <!-- 指定JDK -->
  <string>-vm</string>
  <string>/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home/bin/java</string>

  <!-- 原有内容 -->
  <string>-keyring</string>
  <string>~/.eclipse_keyring</string>
</array>
```

### 参考链接

1. [MAT工具分析Dump文件（大对象定位） - RollBack2010 - 博客园](https://www.cnblogs.com/rb2010/p/14741674.html)
2. [Java中9种常见的CMS GC问题分析与解决](https://tech.meituan.com/2020/11/12/java-9-cms-gc.html) -- 学习资料(复杂)
3. [理解CMS GC日志](https://www.jianshu.com/p/ba768d8e9fec) -- 学习资料(一般)
4. [JVM GC日志分析及性能优化 - 吴振照 - 博客园](https://www.cnblogs.com/wuzhenzhao/p/12486840.html) -- 学习资料(简单)