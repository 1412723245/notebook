# <center>markdown语法指南</center> #
## 基本技能 ##
### 代码 ###
如果你只想高亮语句中的某个函数名可以这样子做
``` 
$(document).ready(function () {  
    alert('hello world');  
});
 ```
<h1 id='1'>跳</h1>
# 标题一 #

## 标题二 ##

# 标题一 #
### 标题三 ###
<font color=red size=5>字体</font>
*斜体文本*
**粗体**
##链接##
[百度](http://baidu.com)
<http://baidu.com>
##网址链接##
[Google][1]
[Yahoo!][yahoo]
[1]:http://www.baidu.com
[yahoo]:http://freebuf.com

##列表##
###普通无序列表###
- 列表文本前使用
+ 列表文本
* 列表文本
###有序列表###
1. test1
3. test2
4. test3
###列表嵌套###
---
1. 列出所有的元素
	- test1
	- test2
	1. dsad
	2. 2323
		2. dasdsd
	- 列表里面引用
	```
	dasdsdadasd
	```
##引用##
>引用文本  
>打点  
>大时代撒的   

![百度](https://www.baidu.com/img/bd_logo1.png)
###换行(末尾两个空格)###
dddddd  
dddddddd
<pre>
def aa():
	dasd
</pre>

[优酷](http://v.youku.com/v_show/id_XMzUyNzkyNzM3Ng==.html?spm=a2hww.20027244.ykRecommend.5~5!2~5~5~A)

$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a}. $$

$$
x \href{why-equal.html}{=} y^2 + 1
$$

$$ (x+1)^2 = \class{hidden}{(x+1)(x+1)} $$
GEEK 们，玩起来！

---
> 
<pre>dd
dasdasdsadas
</pre>

----------

```
dad
    ddasd
```

* [1.语法示例](#1)
	* [1.1test11](#1.1)
	* [1.2test12](#1.2)
