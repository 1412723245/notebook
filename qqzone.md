## f11关于qq爬虫的公开内容，此文档用于技术分享，让大家对爬虫有初步的了解，有问题联系我，大家自己去试试吧，因为我的原因为未能录成视频，抱歉了各位兄弟。##

```
微信名称：码痴
微信公众号：菜鸟ai（等忙完这段时间，会分享一些ai相关的内容，还有爬虫，数据分析的）
```
---
```
所谓爬虫就是访问网页，或者叫做文档，并且从文档中提取我们需要的内容。很多网站是有反爬机制的，有机会可以给大家分享一波反爬的知识。等忙完这段时间出一个关于爬虫的系统的教程吧。现在不是分布式爬虫都不好意思说话了，本人在此处献丑了，代码也没有好好组织，强迫症程序员见谅。
```

## selenium基础知识 ##
```
selenium 是一个测试工具，在爬虫中能帮助我们解析js。
有些页面是有js代码的，但是我我们在使用urllib和requests库的时候是不具有解析js的能力的，所以这个时候我们可以借助这个库来实现。
notice：
	使用selenium的时候，我们相当于在程序中获得了浏览器的对象。
    官方文档:http://selenium-python.readthedocs.io/api.html#module-selenium.webdriver.common.action_chains
```
**环境**
> python3
> selenium库
> selenium的浏览器驱动

### 构建浏览器对象 ###
```
from selenium import webdriver
browser=webdriver.Firefox(executable_path="/home/crazyer/ml/tools/geckodriver")
```
### 访问一个网站 ###
```
browser.get("http://baidu.com")
```
### 定位元素 ###
1. [通过id](#1)
2. [通过css选择器](#2)
3. [通过xpath语法](#3)

<pre id="1">
btn_query=browser.find_element_by_id("kw")
btn_query.send_keys("laolisafe.com")
btn_confirm=browser.find_element_by_id("su")
btn_confirm.click()
</pre>
<pre id="2">
btn_query_css=browser.find_element_by_css_selector(".s_ipt")
btn_query_css.send_keys("laolisafe.com")
btn_confirm_css=browser.find_element_by_css_selector(".bg.s_btn")
btn_confirm_css.click()
</pre>
<pre id="3">
btn_query_xpath=browser.find_element_by_xpath("//input[@class='s_ipt']")
btn_query_xpath.send_keys("laolisafe.com")
btn_query_xpath=browser.find_element_by_xpath("//input[@class='bg s_btn']")
btn_query_xpath.click()
</pre>
### 获取元素的属性 ###
```
方法：get_attribute()
```
### 获取元素的文本内容 ###
```
get_text()
```
### 等待 ###
```
因为网站加载需要一定的时间，所以有时候我们需要等待网站加载好
wait = WebDriverWait(browser, 10)
wait.until(EC.presence_of_element_located((By.ID, 'q')))
上述代码的意思是等待某个元素出现，如果10s还没出现则抛出异常
```
### frame ###
```
一个网页中嵌入了iframe，注意这个iframe和原先的网页不是一个文档，所以是需要切换的，切换的方式
switch_to.frame
在处理完之后我们可以切回默认文档，切换方式
switch_to_default_content
```
### 处理滚动 ###
```
 browser.execute_script("arguments[0].scrollIntoView();", info)
```

## 数据存储 ##
```
mysql
我们在Python中使用pymysql
```
---
```
我没有去分析qq空间的网页，分析会有点难，也会有点耗费时间，所有采用了最简单的办法，完全的来模拟人的操作来实现所有信息的提取，有时间的可以去分析一下qq空间的网页，找出说说数据的接口，那样子的抓起那个将更有效率。
这个爬虫的整体思路就是模拟人的操作，包括人的点击和输入内容。
```
```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
import pymysql
class lsl:#数据库中对应的表的字段
    username=""
    content=""
    img=""
    time=""
    click_count=""
    comment=""
def insert_record(record):#插入数据库中的操作
    cursor.execute("INSERT INTO qqzone(username,content,img,click_count,comment,time) VALUES(\"%s\",\"%s\",\"%s\",\"%s\",\"%s\",\"%s\")" % (record.username,
                   record.content,record.img,record.click_count,record.comment,record.time))
    cursor.connection.commit()
connect=pymysql.connect(host="127.0.0.1",user="root",passwd="",db="qqkongjian",charset="utf8")#连接数据库，方便后边数据处理的时候插入
cursor=connect.cursor()#pymysql的操作可以基于游标，可以看做一个句柄
browser=webdriver.Firefox(executable_path="E:\\geckodriver.exe")#构建浏览器对象
browser.get("https://qzone.qq.com/")#访问qq空间
frame=browser.find_element_by_tag_name("iframe")#找到登录的iframe，可以审查元素
browser.switch_to.frame(frame)#切换到iframe中
dl=browser.find_element_by_id("switcher_plogin")#因为qq空间有多种登录方式，现在选择账号密码登录
dl.click()
u=browser.find_element_by_id("u")
u.send_keys("")#定位账号输入框，并且登录
p=browser.find_element_by_id("p")
p.send_keys("")#定位密码输入框，并输入密码
confirm=browser.find_element_by_id("login_button")
confirm.click()#点击登录
browser.switch_to_default_content()#注意此处必须是切回原先的frame
time.sleep(3)#此处采用的硬等待，大家可以试试上面讲的等待，还有隐式等待，但是貌似会出bug
ss=browser.find_element_by_id("layBackground").find_element_by_class_name("menu_item_311").find_element_by_tag_name("a")
ss.click()#定位说说并且点击
time.sleep(3)
frame1=browser.find_element_by_id("layBackground").find_element_by_id("pageApp").find_element_by_id("app_container").find_element_by_tag_name("iframe")
browser.switch_to.frame(frame1)#说说的内容是在一个iframe中的，此处我们定位到说说所在的frame
ul_obj=browser.find_element_by_id("msgList")#获取消息的ul对象
time.sleep(2)
total_page=browser.find_element_by_id("pager").find_element_by_xpath("//a[@title='末页']").text#获取说说的页数
click_count=0
target = browser.find_element_by_id("pager")
browser.execute_script("arguments[0].scrollIntoView();", target)#滚动页面，只有滚动的时候说说回复才会出现
for i in range(2,int(total_page)+1):#遍历所有的页面
    time.sleep(3)
#    id="pager_num_"+str(click_count)+"_"+str(i)
    infos=ul_obj.find_elements_by_tag_name("li")#获取消息所在的li
#    browser.execute_script("window.scrollTo(0,document.body.scrollHeight)")
    time.sleep(3)
    for info in infos:#遍历所有的消息
        print("insert record.........")
        record=lsl()#将要的信息封装到对象里面
#        target = browser.find_element_by_id("pager")
        
        try: 
            browser.execute_script("arguments[0].scrollIntoView();", info)#每次读取说说的时候就去滚动到相应的地方
            time.sleep(0.1)#等待加载
            record.username=info.find_element_by_class_name("bd").find_element_by_tag_name("a").text.strip()
            record.content=info.find_element_by_class_name("bd").find_element_by_tag_name("pre").text.strip()
            record.time=info.find_element_by_class_name("ft").find_element_by_class_name("info").find_element_by_tag_name("span").text.strip()
            record.click_count=info.find_element_by_class_name("ft").find_element_by_class_name("op").text.replace("更多","").strip()
            record.comment=info.find_element_by_class_name("mod_comment").text.replace("我也说一句","")
            print(record.comment)
            try:
                record.img=info.find_element_by_class_name("md").find_element_by_css_selector(".img-attachments-inner.clearfix").find_element_by_tag_name("a").get_attribute("href").strip()
                insert_record(record)#获取内容并且插入
            except:
                record.img=""
                insert_record(record)
                continue
        except:
            continue
    time.sleep(5)
    next_page=browser.find_element_by_id("pager").find_element_by_xpath("//a[@title='下一页']")
    next_page.send_keys(Keys.ENTER)#注意这里点击是不起作用的，此处使用回车来实现点击
    time.sleep(3)
    print(browser.find_element_by_id("pager").find_element_by_xpath("//a[@title='下一页']").id)
    click_count+=1
```