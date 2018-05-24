## 基于flask的微信开发 ##
最近因为个人的原因，公众号也发的比较少，今天刚刚好有时间，来扑腾一下。
前段时候刚刚好对F11的公众号进行简单的开发，使用的是webpy这个框架，确实轻巧，也容易上手，但是后来了解到作者已经自杀，框架也没有人在维护了，所以就放弃了。不过还是致敬作者。这次打算把原先开发的内容迁移到flask上，据说也是比较轻巧，且扩展性非常好，所以有了这个文章，让我们一起来走进flask的世界。
### flask的安装 ###
pip install flask
但是我们建议是在虚拟环境下去安装，这样子才不会污染全局的环境，造成一些不必要的错误。
### 最小的应用 ###
额外说一些，其实flask用来构建web应用，web应用是基于http协议的。换句话说，我们可以这样子认为，我们创建页面，通过http协议进行网络之间的传输，实现信息的共享。
测试代码：
```
from  flask import  Flask
app=Flask(__name__)
@app.route("/")
def index():
    return "hello,world"
if __name__=="__main__":
    app.run()
```
解析：
可以将Flask看成是我们开发的程序，@app.route设置路由，所谓的路由就是将某个url路径映射到哪一段代码上。像上边，我们将/映射到index函数上，这个模式和.net非常的相似。其实index函数是帮助我们生成和定制http协议中的回包的，这样理解更好一些。
app.run让我们的代码跑在本地的服务器上，是flask内置的服务器。
调试模式:
	我们可以在启动的时候指定服务器为调试模式，调试模式下，当我们修改代码后，服务器会自定的重新进行加载。
设置的方式：
	app.run(debug=True) or app.debug=True app.run()
#### 路由 ####
路由的设置是通过装饰器来实现的，装饰器可以实现横向切入，即在运行这个函数的时候，可以先运行其他的代码。像.net里面的注解。
#### 在路由里面指定变量规则 ####
给url添加变量的规则是<变量类型:变量名>
变量类型有：int float path(输入的部分是路径)
测试代码：
```
@app.route("/hello/<username>")
def hello(username):
    return "hello %s" % username
@app.route("/hello1/<int:c>")
def count(c):
    return "you entered %d" % c
@app.route("/hello2/<path:p>")
def hello2(p):
    return "the path is %s" % p

```
#### 生成url ####
url_for函数生成url
这个函数的第一个参数是一个函数的名字，第二个可以传递参数
#### 设置http方法 ####
通过设置app.route()这个函数的methods参数，我们可以让函数支持相应的http方法，methods参数是一个list，我们可以传相应的方法进去
测试代码
```
@app.route("/testzz",methods=["GET","POST"])
def testzz():
    if request.method=="GET":
        return "the get"
    elif request.method=="POST":
        return "the post"
    else:
        pass
```
#### 静态文件的支持 ####
我们会在网页中引用一些css或者是一些图片之类的东西。
这里使用惯例，我们可以将静态的内容放在static目录下，并且路由系统已经为我们做好了这个映射，我们可以使用static/文件名来访问
#### 模板渲染 ####
我们使用Python来生成html是一件很苦逼的事情，很繁琐，也很麻烦，所以我们这里使用模板引擎来生成前端的HTML页面。flask中配备了jinjia2模板引擎。
测试代码：
```
@app.route("/testtemplate")
@app.route("/testtemplate/<username>")
def testtemplate(username=None):
    return render_template("test.html",username=username)
```
```
<!doctype html>
<html>
    <title>hello from flask</title>
    {% if username %}
    hello {{username}}
    {% else %}
    hello,world
    {% endif %}
</html>
```
#### 访问请求数据 ####
获取请求数据，我们通过请求对象request
获取GET提交的数据 request.args.get()
获取POST或者PUT提交的数据 request.form[""]
代码测试：
```
def login():

    if request.method=="GET":
        username=request.args.get("username")
        passwd=request.args.get("passwd")
        return render_template("login.html",m="GET METHOD",username=username,passwd=passwd)
    elif request.method=="POST":
        username=request.form["username"]
        passwd=request.form["passwd"]
        return render_template("login.html",m="POST METHOD",username=username,passwd=passwd)
    else:
        return "error"
```
#### 文件上传 ####
上传到服务器端的文件，我们可以通过request.files这个对象来获取
测试代码：
```
@app.route("/upload",methods=["GET","POST"])
def upload():
    if request.method=="POST":
        f=request.files["test"]
        f.save(werkzeug.secure_filename(f.filename))
        return "ok"
    elif request.method=="GET":
        return  render_template("upload.html")
    else:
        return  "error"
```
notice:
	secure_filename这个函数可以为程序提供安全性，保存前确保使用这个函数，这个函数在werkseug这个库里面
#### cookie解析 ####
cookie是用来跟踪用户的。flask中访问和设置cookie可以通过cookies和set_cookie来实现。
设置cookie
通常我们在在视图函数中返回一个视图，其实也是相当于返回一个字符串的，只不过这个字符串的内容是html内容，flask帮助我们封装了response的包，我们可以自己来定制这个response，通过flask中的make_response这个函数。设置cookie是需要设置响应体的，所以需要我们定制response。
##### 设置cookie #####
测试代码
```
@app.route("/setcookie")                 
def setCookie():                         
    resp=make_response()                 
    resp.set_cookie("username","zhanggd")
    return resp                          
```
##### 获取cookie #####
从客户端接受到的请求包的内容会被flask封装进request这个对象内，我们通过这个对象来获取cookie
测试代码
```
@app.route("/getcookie")         
def getCookie():                 
    c=request.cookies["username"]
    return c                     
```
#### 重定向和错误 ####
##### 重定向 #####
当访问一个url的时候，跳转到另一个url对应的位置。
整个过程为：
	当请求这个url的时候，返回一个code为302的包，然后浏览器会访问302中location中指定的url
测试代码：
```
@app.route("/")                      
def index():                         
    return redirect(url_for("test")) 
```
##### 错误 #####
当浏览器访问非法的页面，或者是提交了非法的内容，我们将要返回给他错误的页面。
返回一个401
测试代码
```
@app.route("/test2")
def test3():        
    abort(401)      
```
自定义404页面（同样通过abort进行跳转）
```
@app.errorhandler(404)                     
def page_not_found(error):                 
    return render_template("404.html"),404 
```
#### 关于响应 #####
我们在视图函数里面可以返回三种类型的值：
	字符串：如果返回的是字符串，flask直接封装进回包的体内
    元祖：如果是元祖的话，flask会根据你设置的相应内容来设置相应的回包，元祖的格式为（视图，status，headers）headers可以是list或者是字典
    响应对象：如果是直接响应对象，直接根据这个对象来构造
操作回包代码测试：
```
@app.route("/test3")                                   
def test4():                                           
    resp=make_response(render_template("404.html"),404)
    resp.headers["test"]="test"                        
    return resp                                        
```
#### 会话 ####
允许在不同请求之间存储用户的信息。
flask中使用session，这个是session是基于cookie的
我们需要设置一个秘钥：app.secret_key=""
session为一个字典，设置值session["username"]=username
删除session中的值，session.pop("username")