## 基于flask的web开发 ##
### 1.1初始化 ###
初始化，flaskweb开发必须先创建一个程序实例，web服务器使用一种叫做wsgi的协议，将所有的客户端的请求都转交给这个对象来处理。
代码实例：
```
fron flask import Flask
app=Flask(__name__)
flask根据__name__这个参数来确定主模块
### 1.2路由和视图 ###
处理url和代码之间的映射就叫做是路由
notice：
Python的装饰器
类似与注解，都是为了解决横向切入的问题。
```
def testdemo(func):
    def wrapper(*argv,**kwargv):
        print("start")
        func(*argv,**kwargv)
        print("end")
    return wrapper

@testdemo
def test(a,b):
    print("a + b is :%d" % (a+b))
#a=testdemo(test)
#a(2,3)
test(2,3)
### 程序和请求上下文 ###
在我们视图函数里面，我们需要访问一些与请求相关的对象，在flask中的做法是为每一个请求封装一个request对象，但注意不是全局的，因为在多个线程中，每个request对象是不同的。
flask有两种上下文：
程序上下文
	current_app当前激活程序的实例
    g处理请求时用作临时存储的对象
请求上下文
	请求对象，封装了客户端发出http内容
    用户会话

```