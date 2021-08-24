##### 磁盘查找
**搜索/home目录下名称是xxx的文件** <br>
```find /home -name xxx```

**磁盘大小** <br>
```df -h```

**分区** <br>
```fdisk -l```

**挂载** <br>
```mount /dev/sda2 /device```

##### shell实现数组追加
```shell
msg=()
for method_item in ${method_items[@]}
do 
    $method_item
    item_msg="$?"
    msg[${#msg[@]}]=$item_msg # #msg[@]表示数组长度
done
```
##### shell实现split
```shell
#!/bin/bash  
  
str="hello,world,i,like,you,babalala"  
arr=(${str//,/ })  
  
for i in ${arr[@]}  
do  
    echo $i  
done  
```
##### 控制ping命令次数、超时、间隔
```shell
# -w 超时，单位秒
# -c 次数，10次停止
# -i 间隔，单位秒
ping -w 0.5 -c 10 -i 1 www.alibaba.com
```
##### 软链接
`ln -s $source_path $dest_path`

#####  别名
`alias ls='ls --color=auto'`
alias的作用仅在该次登入的操作，即输入一次alias后，这个修改只在当前的Shell生效。如果重新开启一个 Shell，或者重新登录，则这些alias将无法使用。好在linux中提供alias永久化的方法：
- 若要每次登入就自动生效别名，则把别名加在/etc/profile或~/.bashrc中。然后# source ~/.bashrc
- 若要让每一位用户都生效别名，则把别名加在/etc/bashrc最后面，然后# source /etc/bashrc

##### 循环操作数组
```shell
IMAGE_NAMES=("java" "python" "cpp")
IMAGE_TAG=1.0

LEN=${#IMAGE_NAMES[@]}

for ((i=LEN-1; i>=0; i--))
do
    IMAGE_NAME=${IMAGE_NAMES[$i]}
    echo "docker rmi registry.cn-beijing.aliyuncs.com/zhangchx/${IMAGE_NAME}:${IMAGE_TAG}"
    docker rmi registry.cn-beijing.aliyuncs.com/zhangchx/${IMAGE_NAME}:${IMAGE_TAG}
done

for IMAGE_NAME in ${IMAGE_NAMES[@]}
do
	echo "docker pull registry.cn-beijing.aliyuncs.com/zhangchx/${IMAGE_NAME }:${IMAGE_TAG}"
	docker pull registry.cn-beijing.aliyuncs.com/zhangchx/${IMAGE_NAME }:${IMAGE_TAG}
done
```

##### 字符串匹配：正则表达式
使用grep -E 正则表达式来匹配想要的东西，如版本号
https://www.cnblogs.com/harryblog/p/8087783.html