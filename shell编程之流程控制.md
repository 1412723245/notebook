## shell编程之流程控制 ##
```
概述：
	流程控制在每一门编程语言里面都有，无论是低级语言还是高级语言，这个是我们日常中最基础的操作，在shell脚本中也不例外，今天我们谈谈流程控制。
```
### shell之if ###
```
语法结构：
	if condition1
	then
		conmand1
		command2
		...
	[elif] condition2
	[then]
		command3
		...
	[else]
		command4
		...
	fi
notice:
	跟其他语言的if差不多，可以把每一块代码，看成是一个分支，这个字每个流程就很清楚了。当condition1满足的话，就去找他后边的then中的代码块去执行，如果condition1不满足，再去看下一个分支。都不满足执行else
```
代码测试：
```
我们以查找某个用户为例进行测试：
首先查看下我们系统的用户，全部的用户是放在/etc/passwd这个文件的，我们将用户名筛选出来：
cat /etc/passwd | awk -F: '{print $1}'
	命令解释：
		awk一个文本处理的，感兴趣的可以去学一下，我这里介绍我里面有的
		-F: 指明:作为分隔符
		$1： 代表的是分隔后的第一个域
我们在找一下我们要测试的用户test,只要将上述的命令通过管道给grep就好了：
cat /etc/passwd | awk -F: '{print $1}' | grep "test"
	命令解释：
		grep帮助我们筛选出含有test的行
我这里是什么都没有输出的，之后创建我们的脚本如下：
```
测试代码：
```
#!/bin/bash
if cat /etc/passwd |grep "test"
then
        echo "test user exists"
else
        echo "no this user"
fi
notice：
	注意默认的是没有权限的，先加权限在执行
```
运行代码：
```
结果：
	no this user
```
现在我们开始创建用户：
```
useradd test #请确保你有足够的权限
之后我们在通过上边的那个用户查看下当前用户是否有test:
cat /etc/passwd | awk -F: '{print $1}' | grep "test"
结果：
	test（说明当前有这个用户了）
```
我们在运行我们的脚本，结果如下：
```
test user exists
```
到此if语句介绍结束
---
### shell编程之for循环： ###
```
语法如下：
for item in a b c d e
do
command
done
研究一下for：
	首先我们看到in后边是空格分隔的一系列字符，我们想大概是用空格符分隔的一个list，我们可以去猜想是不是只要是空白符就
```