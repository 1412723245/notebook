## shell编程基础 ##
```
注意运行的时候给sh脚本添加可执行权限，默认是没有的。
```
### shll变量 ###
```
显示声明：
var1=value
语句赋值：
for file in `ls /etc/`
notice:
一个特殊的变量IFS，这个变量纪录了默认的分隔符，
默认分隔符是空白符
```
测试代码：
```
#!/bin/bash
test="a b c d"
for a in $test
do
echo $a
done
test1="a
b
c
d"
for b in $test1
do
echo $b
done
```
### 获取变量的值 ###
```
$变量名
${变量名}
```
### 只读变量 ###
```
readonly申明只读变量，只读变量顾名思义就是变量的值是不能修改的。
var1="test"
readonly var1
```
测试代码
```
#!/bin/bash
var1=test
readonly var1
var1=zz
```
### 删除变量 ###
```
unset 变量名
notice:
变量被删除后不能再次使用，而且也不能删除只读变量。
```
测试代码：
```
#!/bin/bash
var1=test
echo $var1
unset var1
echo $var1
var2=testzz
readonly var2
unset var2#报错
echo $var2
```
### shell字符串 ###
```
shell中的字符串可以用单引号或者是双引号进行包裹，或者是不进行包裹
单引号和双引号区别是：
	单引号里面的字符串是原样输出的，双引号里面是可以有变量转义字符的，
单引号和双引号都是不能签各自嵌套的
```
测试代码：
```
#!/bin/bash
var1='testzz'
echo 'aaaa\n${var1}'
echo -e "aaaa\\n${var1}\n"
notice:
	-e是让echo命令启用转义的意思
	-n让echo最后不输出换行
```
### 字符串的拼接 ###
```
shell中的拼接比较特殊，直接将字符串写一块就可以
```
测试代码：
```
#!/bin/bash
var1=testzz
echo "string1"$var1"string2"
```
### 获取字符串的长度 ###
```
${#变量名}
```
测试代码：
```
#!/bin/bash
var1=testzz
echo ${#var1}
```
### 提取子字符串 ###
```
${变量名:开始的索引:提取的字符串的长度}
```
测试代码:
```
#!/bin/bash
var1="hello world,test"
echo ${var1:6:5}#此处取的是world
```
### 查找子字符串 ###
```
`expr index "testzz" zz`输出zz的索引但是注意是从一开始计算的，如果找不到子串的话
输出为0
notice:
	expr经常是用于数值运算，但是也可以用于字符串的运算
```
测试代码：
```
#!/bin/bash
var1="hello world"
echo `expr index "$var1" wo`#注意此处的变量需要用双引号包裹，如果没有的话会将其和后边的子串进行拼接，
#造成语法错误
```
### shell数组 ###
```
数组名=(val1 val2 val3 ...)以空白符进行分割
数组名=(val1
val2
val3
val4)
还可以定义每一个分量如下:
arr1[0]=a
arr1[1]=b
arr1[n]=test
notice：
	bash支持一维数组，不支持多维数组，也是按照索引来取值的，下标是从0开始的，
下标可以是整数或者是算术运算符
```
测试代码：
```
#!/bin/bash
arr1=(a b c d e f)
arr2=(a
b
c
d
e
f)
arr3[0]=a
arr3[1]=b
arr3[2]=c
echo ${arr1[*]}
echo ${arr2[*]}
echo ${arr3[*]}
```
### 读取数组 ###
```
${数组名[index]}输出指定下标的元素
${数组名[@]}输出全部元素
```
测试代码：
```
#!/bin/bash
arr=(aa bb cc dd)
echo ${arr[0]}
echo ${arr[@]}
echo ${arr[8]}
```
### 获取数组的长度 ###
```
${#数组名[@]}获取整个数组长度
${#数组名[*]}获取整个数组长度
${#数组名[index]}获取数组某个元素的长度
```
测试代码:
```
#!/bin/bash
arr1=(aa bb cc ddd)
echo ${arr1[*]}
echo ${arr1[@]}
echo ${#arr1[*]}
echo ${#arr1[@]}
echo ${#arr1[1]}
```