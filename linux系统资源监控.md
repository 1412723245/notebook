## 简单模型，实现的客户机监控 ##
```
问题来源，受朋友所托，实现一个设备监控的脚本，其实是有些成熟的软件可以用的，但是考虑到最近在学linuxshell脚本，而且项目要求只是演示目的，所以就自己来实现了。
```
** 基本的思路 **
> 1.设置定时脚本，通过shell脚本导出服务器的状态信息
> 2.状态改变的时候，通过http协议将信息回传到服务器端
> 3.服务器接收到状态后，更新状态并进行数据的可视化

### 定时监本 ###
```
本打算使用定时脚本的，但是发现crontab的最小粒度是分钟，所以放弃，改用延迟的机制。设置开机自启脚本，然后在脚本中获取客户机的状态，之后回传。
```
定时监本实现：
```
#!/bin/bash
while true
do
sleep 2
command#在这里面我们获取服务器状态，并且导出到csv 
done
```
获取ip地址：
```
ifconfig | grep 'inet'| grep -v '127.0.0.1' | cut -d: -f2 |awk '{print $1}'
notice:
	grep:（文本查找）
		 -v 是过滤掉某些行，这里我们把环回地址过滤掉
	cut:（文本切分）
		-d 是指定分隔符
		-f 是取那一部分的意思,f2是取第二部分的意思
	awk:(进行文本处理)
		默认是以空格来分隔的，切割后$0代表所有域，$1代表第一个域		
```
查看内存情况:
```
获取总内存大小，空闲内存和可利用内存大小
cat /proc/meminfo | grep -E "MemTotal|MemFree|MemAvailable" >a.txt
```
查看各个分区使用情况
```
df -lh > a.txt
```
查看cpu使用情况
```
top -n 1 | grep Cpu
```
------
我们将信息发送到服务器端
```
curl命令
我们将所有的文本信息发送到服务器端，我们使用curl发出post请求：
curl -d "param1=value1&param2=value2" url
```
完整脚本：
```
#!/bin/bash
while true
do
cat /proc/meminfo | grep -E "MemTotal|MemFree|MemAvailable" >a.txt
df -lh >> a.txt
top -n 1 | grep Cpu >>a.txt
info=$("cat a.txt")
curl -d "info=${info}" http://127.0.0.1
sleep 2
done
```
## 服务器端 ##
```
使用了Django，写一个controller，等待客户端传数据上去，然后接收数据，并且传入数据库中。
```
伪代码：（鄙人不会Django）
```
def getInfo():
	ip=
	mem=
	cpuinfo=
	入库
之后在前端进行可视化。
```