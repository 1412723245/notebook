## 使用Python实现发送邮件 ##
邮件发生的过程：
我们在本地，例如qq里面写好一篇邮件的时候，这里以qq示例，点击发生，首先邮件会被发送到qq的邮件服务器，这里发送邮件使用的协议是smtp，所谓协议就是通信的规则，想深入了解的可以看看具体的协议格式，qq邮件服务器收到邮件后，首先判断邮件的目的地址是不是自己域的，如果是的话将邮件存储在自己的服务器里面，如果不是的话，进行邮件的转发，直到到达所在邮件服务器。（这里比如说是发送到163服务器，163服务器会收到我们的邮件，并且存储，相当于是邮局），当我们发送的目的用户登录自己邮箱的时候，使用pop3协议向163邮件服务器提取自己邮件信息，这样子他就可以看到自己的邮件了。
### 涉及的协议 ###
smtp：简单邮件传输协议，用来发送邮件
具体了解的来这里：https://tools.ietf.org/html/rfc1869
pop3：用来接收邮件
具体了解的来这里：https://tools.ietf.org/html/rfc1939
### 使用Python进行发送邮件 ###
我们这里以qq示例，需要在qq邮件里面进行设置，拿到授权码，不会的自行百度
```
import smtplib
from email.mime.text import MIMEText
from email.header import Header
#MIMEText这是一个邮件的正文，邮件的正文是有From To Subject 正文这四个部分
message=MIMEText("这是一个测试","plain","utf-8")
message["From"]="###"
message["To"]="1412723245@qq.com"
message["Subject"]="这是一个测试"
s=smtplib.SMTP_SSL("smtp.qq.com",465)
s.set_debuglevel(1)#设置这个后可以在控制台看到，协议的收发过程
try:
    s.login("账号","这里是开启之后的授权码")
    s.sendmail("来自谁","发给谁",message.as_string())
except smtplib.SMTPException:
    print("error")
s.quit()
```
邮件会被放在垃圾箱，如何绕过，请求支援