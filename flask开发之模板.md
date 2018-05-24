## 模板 ##
### jinja2配置 ###
jinja2的默认配置为：
	所有扩展名为.html .htm .xml .xhtml模板会开启自动转义
    模板可以通过{% autoescape%}标签选择自动转义的开关
    flask在jinja2上下文中插入了几个全局函数和助手
### 标准上下文 ###
我们可以在jinja2模板里面使用这些对象
config request session g url_for get_flashed_messages
### 标准过滤器 ###
tojson
	将给定的对象转换为json表示
safe
	认为输入安全不进行转义
### 控制自动转义 ###
自动转义是指文本中的<> " '之类的字符会被转义为html实体
对于不需要转义的的html代码段，我们可以通过三种方式实现
	在传到模板前，我们将其封装为Markup对象
    通过过滤器safe禁止转义，信任这段代码
    通过关闭autoescape{{ autoescape flase }}
### 注册过滤器 ###
我们可以注册一些过滤器，来在jinja2模板里面来使用
注册的方式有两种
	装饰器方式
    	@app.template_filter("reverse")
        def revers_filter():
        	return 
    通过jinja——env
    	def revers_filter():
        pass
        app.jinja_env.filter["reverse"]=revers_filter
### 上下文处理器 ###
通过上下文处理器，我们可以给模板传递变量，或者是函数
传递变量
	@app.context_processor
    def inject_user():
    	username="zs"
    	return dict(user=username)
传递函数
	@app.context_processor
    def test():
    	def a():
        	return "test"
        return dict(func=a)
传递之后，我们就可以在模板里面使用