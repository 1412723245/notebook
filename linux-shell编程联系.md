## 将一个目录下的所有文件扩展名改为bak ##
```
思路：
	修改名字我们可以使用mv命令
	mv 源文件 目标文件
```
测试代码：
```
我们首先创建一个test目录：mkdir test
我们在创建三个文件：touch aa bb cc
开始编写脚本：
#!/bin/bash
for file in `ls /root/sh/test`
do
echo $file
mv /root/sh/test/$file /root/sh/test/$file".bak"
done
```
## 找出一个目录下的最大的文件 ##
```
遍历目录中的所有文件，找出最大的文件
ls -l会列出文件的大小，我们在这里面进行提取
```
测试代码：
```
xfilename=""
for file in *.*
do
#echo $file
size=$(ls -l $file | cut -d" " -f5)
#echo $size
if [ $size -lt $maxsize ]
then
        maxsize=$size
        maxfilename=$file
fi
done
echo $file" is the max file,size:$maxsize"
```
notice:
	因为多次接触到cut命令，所以我们学习一波。
	cut:
		将文件中的每一行字符进行剪切，选取我们要的每一部分
		参数：
			-[n]b：以字节为单位进行切分，这些字节位置将忽略多字节边界，除非指定了n
			-c:以字符为单位进行切割
			-d:指定分隔符
			-f:与-d结合使用，指出我们需要的域
## 通过ping命令检测自己c段的主机存活情况 ##
```
首先提取自己的ip：
ifconfig | grep inet|cut -d":" -f2 |cut -d" " -f1| head 
-n 1
之后切出ip的前三个字段：
cut -d"." -f1-3
遍历所有的ip，并且发送ping包
```
测试代码：
```

#!/bin/bash
ip=$(ifconfig | grep inet|cut -d":" -f2 |cut -d" " -f1| head -n 1 | cut -d"." -f1-3)
for i in `seq 254`
do
ping $ip"."$i -c 2 > /dev/null
if [ $? -eq 0 ]
then
        echo $ip"."$i" is active"
else
        echo $ip"."$i" is active"
fi
done
```
## 生成10个随机名字的文件 ##
```
要求：
	包括数字和字幕
思路：
	我们使用uuid来命名文件，读取linux中的uuid
	cat /proc/sys/kernel/random/uuid
```
测试代码：
```
#!/bin/bash
for((i=0;i<10;i++))
do
filename=`cat /proc/sys/kernel/random/uuid`
touch $filename".txt"
done
```
## 用一个操作mysql的内容来结尾吧 ##
```
操作mysql是通过mysql的-e这个命令来实现
接下来我们测试一下查询命令，其他命令类似
```
测试代码：
```
#!/bin/bash
ss="mysql -uroot -h127.0.0.1  -prootzgd2018! --default-character-set=utf8 -N "
sql="select * from user.user"
result=$($ss -e "$sql")#这里必须包含在""中，把整个命令看成一个参数
echo -e "$result"#这里必须加双引号，才能正确的换行
```