Redis       MongoDB    nosql --> not  only sql   不是唯一sql

web: 万维网

HTTP 协议   HyperText   T  P   超级文本的传输协议  底层是TCP协议

+ 构建在长连接基础上的短连接协议 

HTML    HyperText M L

TCP：

+ 建立连接：三次握手

+ 断开连接：四次挥手

+ 可靠

+ 长连接协议    区别于短连接UDP   相当于打电话，视频聊天，一直持续



```python
#!/usr/bin/env python

import socket
import time

addr = ('127.0.0.1', 8000)
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # 创建 socket 对象
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)  # 为 sock 打开地址可重用选项
sock.bind(addr)   # 绑定服务器地址
sock.listen(100)  # 设置监听队列

# 定义 "响应报文"
template = '''
HTTP/1.1 200 OK

<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />

        <title>Seamile</title>

        <style>
            h1, p {
                text-align: center;
                font-size: 2.5em;
            }
            .avatar {
                border-radius: 20px;
                box-shadow: 5px 5px 20px grey;
                width: 500px;
                margin: 0 auto;
                display: block;
            }
        </style>
    </head>
    <body>
        <h1>Seamile</h1>
        <div><img class="avatar" src="https://inews.gtimg.com/newsapp_ls/0/10229330043_294195/0" /></div>
        <p>%s</p>
    </body>
</html>
'''


def get_url(request_str):
    '''从 "请求报文" 中获取请求的 URL'''
    first_line = request_str.split('\n')[0]  # 取出第一行
    url = first_line.split(' ')[1]  # 按空格切分，取出中间的 URL
    return url


while True:
    print('服务器已运行，正在等待客户端连接。。。')

    # 等待接受客户端连接
    # 第一个返回值是客户端的 socket 对象
    # 第二个返回值是客户端的地址
    cli_sock, cli_addr = sock.accept()
    print('接收到来自客户端 %s:%s 的连接' % cli_addr)

    # 接收客户端传来的数据，1024是接收缓冲区的大小
    cli_request = cli_sock.recv(1024).decode('utf8')
    print('接收到客户端发来的 "请求报文": \n%s' % cli_request)

    # 获取用户的 URL
    url = get_url(cli_request)

    # 根据 URL 生成不同的返回值
    if url == '/foo':
        response = template % '爱妃退下，朕在调戏代码'
    elif url == '/bar':
        response = template % '姜伟老师没醉过，但求一醉'
    else:
        response = template % 'hello world'

    print(url, response)
    cli_sock.sendall(response.encode('utf8'))  # 向客户端发送数据

    # 断开与客户端的连接
    cli_sock.close()
    print('连接断开, 退出！')
```

状态码 200成功 404 页面不成功 500 服务器啥的。。

ajax 异步

协程  线程  进程

yeild



web处理方法：

post

delete

put

get

head

patch



常见的局域网地址

10.x.x.x

172.16.x.x - 172.17.x.x

192.168.x.x



0.0.0.0   可以通过127.0.0.1  局域网10.11.55.196访问

相当于自己电脑上所有的ip

localhost  



URL包括什么  统一资源定位  定位网站的各种资源

http://xxx.com:80/foo.bar.xxx.html?aaa=123&bbb=456#top   协议

https

ftp

socks

redis

mysql://xxxx

# Web服务器与Tornado入门

## 一、HTTP服务器的真相

HTTP协议是建立在TCP协议之上的短连接协议。

它利用TCP协议的可靠性，用来传输超文本（HTML），通信一次连接一次，通信完成后TCP连接关闭。

所以如果想创建一个HTTP Server需要通过Socket搭建一个服务端程序。

### 1.最简单的HTTP Server

```python
import socket

addr = ('0.0.0.0',80)

response = b'''
http/1.1 200 OK

<!DOCTYPE heml>
<html>
 	<head>
 		<title>Hello</title>
 	</head>
 	<body>
 		<h1 style="text-align: center;">Hello World</h1>
 	</body>
</html>
'''

listen_socket = socket.socket() #建立SOCK连接
listen_socket.bind(addr) #绑定 IP：端口
listen_socket.listen(100) #开始监听

print('Server is running: %s:%s ...' % addr)

while True:
    client_socket,client_address = listen_socket.accept() # 接受客户端的连接
    print('client request from %s:%s' % client_address)
    request = client_socket.recv(1024) #接收客户端数据
    http_response = response
    client_socket.sendall(http_response)
    client_socket.close()
```

### 2.对SimpleServer进行扩展

1.根据不同URL显示不同页面

2.页面整体样式不变，根据不同参数，从数据库中取出不同学生信息，并填充到页面中

## 二、Web框架概述

随着技术的发展，我们每天的要处理的信息量都在爆炸新的增加，传统的静态页面技术早已跟不上时代需求，因而催生了动态页面技术。

所谓动态⻚⾯，即所有的⻚⾯⽤程序来⽣成，以细节实现上的不同，⼜可分为“前端动态⻚⾯”和“后端动
态⻚⾯”。
我们在 Web 前端阶段所学 Ajax、VUE 等技术，就是前端动态⻚⾯。⽽今后我们所学的主要是后端动态
⻚⾯技术，甚⾄是两者结合使⽤。

### 1.Web服务器原理

第一小节的SimpleServer虽然代码仅仅30多行，但已经包含了一个小型Web系统的核心。一个完备的Web系统如下：

![1568028271085](C:\Users\miyongqing\AppData\Roaming\Typora\typora-user-images\1568028271085.png)

### 2.常见的Web框架

如果想要完成更复杂的功能，还需要深入开发很多东西，比如模板系统、ORM系统、路由系统、会话机制。。。

好在这些基础且通用的东西已经被很多前辈开发完成，我们没有必须再去造轮子，他们按照自己的用途和想法，将各种系统开发、组合，最终为我们提供了各种各样的Web框架。

| web framework | description                                                  |
| ------------- | ------------------------------------------------------------ |
| Django        | 全能型框架，大而全，插件丰富，文档丰富，社区活跃，适合快速开发，内部耦合比较紧 |
| flask         | 微型框架，适合新手学习，极其灵活，便于二次开发和扩展，生态环境好，插件丰富 |
| tornado       | 异步处理，事件驱动（epoll），性能优异                        |
| bottle        | 单文件框架，结构紧凑，适合初学者阅读源码，了解web原理        |
| web.py        | 代码优美，适合学习源码                                       |
| falcon        | 性能优异适合写API接口                                        |
| quixote       | 一个爷爷级别的框架，著名的豆瓣网用的便是这个                 |
| sanic         | 后期之秀，性能秒杀以上所有前辈，但没有前辈们稳定             |

## 三、Tornado入门

tornado是由friendfeed公司开发的web框架，该公司已经于2009年被 Facebook 收购，原本的FriendFeed 已经成为了 Facebook 的⼀部分。

Tornado 最⼤的特点就是他实现了⼀个 “异步⾮阻塞” 的 HTTP Server，性能⾮常优异。

### 1.安装

```
pip install tornado	
```

### 2.hello world

```python
import tornado.ioloop
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.wrete("hello,wrold")

def make_app():
    return tornado.web.Application([
        (r"/",MainHandler),
    ])

if __name__ == "__main__":
    app = make_app()
    app.listen(8888)
    tornado.ioloop.IOLoop.current().start()
```

### 3.启动参数

```python
from tornado.options import parse_sommand_line,define,options

define("host",default='0.0.0.0',help="主机地址",type=str)
define("port",default=8888,help="主机端口",type=int)

parse_command_line()
print('你传入的host: %s' % options.host)
print('你传入的port: %s' % options.port)
```

### 4.路由处理

```python
import tornado.ioloop
from tornado.web import RequestHandler, Application

class HomeHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("欢迎进入主页")
        
class BookHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("你想看的书应有尽有")
        
app = Application([
    ('/',HomeHandler),
    ('/book/',BookHandler)
])

app.listen(8000)
tornado.ioloop.IOLoop.current().strat()

```

5.处理GET和POST请求

```python
class StoryHandler(tornado.web.RequestHandler):
    stories = {1:'小红帽',2:'匹诺曹',3:'阿拉丁神灯'}
    
    def get(self):
        story_id = self.get_argument('story_id')
        story = self.stories[story_id]
        self.write("你想看 %s 的故事" % story)
        
    def post(self):
    	pass
```



HTTP的请求方法：

| Method  | Description                                                  |
| ------- | ------------------------------------------------------------ |
| POST    | 向指定资源提交数据进⾏处理请求, 数据被包含在请求体中         |
| GET     | 请求指定的⻚⾯信息，并返回实体主体。                         |
| PUT     | 从客户端向服务器传送的数据取代指定的⽂档的内容。             |
| DELETE  | 请求服务器删除指定的⻚⾯                                     |
| HEAD    | 类似于 GET 请求，只不过返回的响应中没有具体的内容，⽤于获取报头 |
| PATCH   | 是对 PUT ⽅法的补充，⽤来对已知资源进⾏局部更新 。           |
| OPTIONS | 允许客户端查看服务器的性能。                                 |

6. 练习
1. 在 MySQL 中建⽴ user 表, 包含字段: id, name, age, sex, city 等, 并插⼊若⼲数据
2. 开发接⼝，并根据⽤户传⼊的 id 显示对应的⽤户信息
3. 开发接⼝，并根据⽤户传⼊的数值修改⽤户数据