# 背景
公司需要开发C++的SDK，所以需要温习大学学过的C++，将SDK实现。
本文章不会对C++的一些语法基础做过多描述，主要聚焦在开发过程中关于指令、调试、错误、技巧等方面的记录。

# 子类调用父类
A是B的父类，fun()是B继承的A的，在B中调用A的fun()则是A::fun()

# G++命令简述
`g++ -c sample.1.cpp` // -c表示编译成o <br>
`gcc -o hello hello.c -I /home/hello/include -L /home/hello/lib -lworld` <br>
上面这句表示在编译hello.c时：<br>
`-I /home/hello/include`表示将/home/hello/include目录作为第一个寻找头文件的目录，寻找的顺序是：/home/hello/include-->/usr/include-->/usr/local/includ <br>
`-L /home/hello/lib`表示将/home/hello/lib目录作为第一个寻找库文件的目录，寻找的顺序是：/home/hello/lib-->/lib-->/usr/lib-->/usr/local/lib <br>
`-lworld`表示在上面的lib的路径中寻找libworld.so动态库文件（如果gcc编译选项中加入了“-static”表示寻找libworld.a静态库文件）<br>
# 动态库·静态库
`静态库文件`一般是以.a为后缀的库文件，它在编译连接时会将库文件的内容全部添加到可执行文件中，在编译连接完成后，静态库文件便不再影响可执行文件。编译连接时，静态库文件搜索目录顺序为：
```shell
1) 编译连接时 -L 参数指定的目录；
2) 环境变量目录 LIBRARY_PATH；
3) 固定目录 /lib、/usr/lib、/usr/local/lib
```

`动态库文件`一般以.so结尾，它在编译连接时只把动态库的文件添加到可执行文件，只在程序运行时才加载库文件。这种方式的优点是非常灵活，如果动态库文件内部有变动，那么只需重要重新编译库文件并刷新`ldconfig`即可。编译连接时，动态库文件搜索目录顺序为：
```shell
1) 编译连接时 -L 参数指定目录；
2) 环境变量目录 LD_LIBRARY_PATH；
3) 配置文件/etc/ld.so.conf中配置的目录
4) 固定目录 /lib、/usr/lib
```

一般我构建好的静态库和动态库放入/usr/local/lib中，之后执行`echo "/usr/local/lib" >> /etc/ld.so.conf`，再刷新`ldconfig`，可执行文件就可以正常运行了
# Makefile
makefile是执行make命令调用的文件，其中包含了对代码的编辑、打包、清理等逻辑。
librdkafka中包含make、make install（将打包后的so安装到/usr/local/lib上，默认系统会在该目录下搜索动态库）

# 排查segmentfault错误（段错误）
先增加排查堆栈日志：http://www.wtango.com/%E6%AE%B5%E9%94%99%E8%AF%AFsegmentation-fault%E4%BA%A7%E7%94%9F%E7%9A%84%E5%8E%9F%E5%9B%A0%E4%BB%A5%E5%8F%8A%E8%B0%83%E8%AF%95%E6%96%B9%E6%B3%95/ <br>
对日志进行分析：https://my.oschina.net/lowkey2046/blog/674964

# kafka cpp client使用
https://www.cnblogs.com/vincent-vg/p/5855924.html