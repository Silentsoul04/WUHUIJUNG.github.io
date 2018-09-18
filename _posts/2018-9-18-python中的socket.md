---
layout: post
title: socket
date: 2018-9-18
categories: blog
tags: [python]
description: 文艺的流氓。
---
### SOCKET模块
#### Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。在设计模式中，Socket其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socket去组织数据，以符合指定的协议。
### 套接字
#### 基于文件类型的套接字家族
 - 套接字家族的名字：AF_UNIX
 - unix一切皆文件，基于文件的套接字调用的就是底层的文件系统来取数据，两个套接字进程运行在同一机器，可以通过访问同一个文件系统间接完成通信

#### 基于网络类型的套接字家族
 - 套接字家族的名字：AF_INET
 - (还有AF_INET6被用于ipv6，还有一些其他的地址家族，不过，他们要么是只用于某个平台，要么就是已经被废弃，或者是很少被使用，或者是根本没有实现，所有地址家族中，AF_INET是使用最广泛的一个，python支持很多种地址家族，但是由于我们只关心网络编程，所以大部分时候我么只使用AF_INET)

### TCP协议和UDP协议

#### TCP:TCP（Transmission Control Protocol）可靠的、面向连接的协议（eg:打电话）、传输效率低全双工通信（发送缓存和接收缓存）、面向字节流。使用TCP的应用：Web浏览器；电子邮件、文件传输程序。

#### UDP（User Datagram Protocol）不可靠的、无连接的服务，传输效率高（发送前时延小），一对一、一对多、多对一、多对多、面向报文，尽最大努力
服务，无拥塞控制。使用UDP的应用：域名系统 (DNS)；视频流；IP语音(VoIP)。

![WUJINLIN](https://s1.ax1x.com/2018/09/18/iZvFNq.jpg)

#### 使用套接字 -- TCP(tcp是基于链接的，必须先启动服务端，然后再启动客户端去链接服务端)

#### server端
```python
import socket
sk = socket.socket()
sk.bind(('127.0.0.1',8898))  #把地址绑定到套接字
sk.listen()          #监听链接
conn,addr = sk.accept() #接受客户端链接
ret = conn.recv(1024)  #接收客户端信息
print(ret)       #打印客户端信息
conn.send(b'hi')        #向客户端发送信息
conn.close()       #关闭客户端套接字
sk.close()        #关闭服务器套接字(可选)
```
#### cilent端
```python
import socket
sk = socket.socket()           # 创建客户套接字
sk.connect(('127.0.0.1',8898))    # 尝试连接服务器
sk.send(b'hello!')
ret = sk.recv(1024)         # 对话(发送/接收)
print(ret)
sk.close()            # 关闭客户套接字
```

#### server端	-- 这是一个聊天的

```python
import socket
# 第一步定义socker
# 第二步绑定自己的端口
# 第三步监听自己的端口
# 第四步接收别人地址和端口
# 第五步接受别人传输过来的值
# 第六步可以发送信息给别人
# 关闭连接
so = socket.socket()
so.bind(('127.0.0.1',7777))
so.listen()
ip, port = so.accept()  # 接收connection连接，address地址
while True:
    ret = ip.recv(4096).decode('UTF-8')
    if  ret == 'bye':
        print('聊天结束')
        print("hello world")
        ip.send(bytes('OK!bye',encoding='UTF-8'))
        so.close()
        break
    else:
        print(ret)
        info = input('>>>>')
        ip.send(bytes(info,encoding='UTF-8'))
# 整个程序的结构就是判断接收的值，是不是bye是的话，那么就会退出聊天。不然的就会继续执行
```
#### client端	-- 这是一个聊天的
```python
import socket
# 第一步定义socker
# 连接端口
# 发送信息
so = socket.socket()
so.connect(('127.0.0.1',7777))
while True:
    info = input('>>>')
    so.send(bytes(info,encoding='UTF-8'))
    ret = so.recv(4096).decode('UTF-8')
    print(ret)
    if ret == 'bye':        # 如果是等于bye 那么就会退出循环，并且会关闭连接
        print('聊天结束')
        so.send(bytes('bye', encoding='UTF-8'))
        so.close()
        break
    else:
        continue
# 整个程序的结构就是判断接收的值，是不是bye是的话，那么就会退出聊天。不然的就会继续执行
```

#### 使用套接字 -- UDP(udp是无链接的，启动服务之后可以直接接受消息，不需要提前建立链接)
#### UDP需要每次发送的时候加上地址
#### server端 
```python
import socket
udp_sk = socket.socket(type=socket.SOCK_DGRAM)   #创建一个服务器的套接字
udp_sk.bind(('127.0.0.1',9000))        #绑定服务器套接字
msg,addr = udp_sk.recvfrom(1024)
print(msg)
udp_sk.sendto(b'hi',addr)                 # 对话(接收与发送)
udp_sk.close()                         # 关闭服务器套接字
```
#### client端
```python
import socket
ip_port=('127.0.0.1',9000)
udp_sk=socket.socket(type=socket.SOCK_DGRAM)
udp_sk.sendto(b'hello',ip_port)
back_msg,addr=udp_sk.recvfrom(1024)
print(back_msg.decode('utf-8'),addr)
```

#### QQ聊天 -- server端
```python
import socket
so = socket.socket(type=socket.SOCK_DGRAM)  # UDP方式的套接字
so.bind(('127.0.0.1', 9999))                # 绑定自己的端口
while True:                                 # 循环接收的各个地方
    msg,addr = so.recvfrom(4096)            # 接收得方法.
    print(addr)
    print(msg.decode('UTF-8'))
    info = input('>>>').encode('UTF-8')
    so.sendto(info,addr)                    # UDP再发送值得时候需要再加上一个地址
so.close()

# udp的server 不需要进行监听也不需要建立连接
# 再启动服务之后之只能被动的等待客户端发送消息过来
# 客服端发送消息的同时还会自带地址信息
# 消息回复的时候需要带上地址
```
#### QQ聊天	-- client1端
```python
import socket
so = socket.socket(type=socket.SOCK_DGRAM)
ip_port = ('127.0.0.1', 9999)
while True:
    info = input('One:')
    info = ('\035[32m来自One的消息: %s\035[0,m' % info).encode('UTF-8')
    so.sendto(info,ip_port)
    mag,addr = so.recvfrom(4096)
    print(mag.decode('UTF-8'))
```
#### QQ聊天	-- client2端
```python
import socket
so = socket.socket(type=socket.SOCK_DGRAM)      # 定义UDP的套接字
ip_port = ('127.0.0.1', 9999)                   # 发送的端口和地址
while True:
    info = input('Two:')
    info = ('\033[32m来自Two的消息: %s\033[0,m' % info).encode('UTF-8')
    so.sendto(info,ip_port)                     # 同样再发送信息的同时也需要发送地址
    mag,addr = so.recvfrom(4096)                # 接收的时候，需要同时接收地址。第一个是值，第二个是地址
    print(mag.decode('UTF-8'))
```
#### QQ聊天	-- client3端
```python
import socket
so = socket.socket(type=socket.SOCK_DGRAM)
ip_port = ('127.0.0.1', 9999)
while True:
    info = input('Three:')
    info = ('\034[32m来自Three的消息: %s\034[0,m' % info).encode('UTF-8')
    so.sendto(info,ip_port)
    mag,addr = so.recvfrom(4096)
    print(mag.decode('UTF-8'))
```

### 设置时间间隔
#### server端
```python
import socket
socket.setdefaulttimeout(10)			# 等待时间
S = socket.socket()
S.bind(('127.0.0.1', 8888))
S.listen(12)
conn, addr = S.accept()
while True:
    ret = conn.recv(4096).decode('UTF-8')
    if ret == 'bye':
        conn.close()
        break
    else:
        print(ret)
        info = input('>>>')
        conn.send(bytes(info, encoding='UTF-8'))
```
#### client端
```python
import socket

C = socket.socket()
C.connect(('127.0.0.1', 8888))
while True:
    info = input('>>>>')
    C.send(bytes(info, encoding='UTF-8'))
    ret = C.recv(4096).decode('UTF-8')
    if ret == 'bye':
        break
    else:
        print(ret)
```
### socket参数详解
`socket.socket(family=AF_INET,type=SOCK_STREAM,proto=0,fileno=None)`

参数  | 说明
------------- | -------------
family  | 地址系列应为AF_INET(默认值),AF_INET6,AF_UNIX,AF_CAN或AF_RDS。(AF_UNIX 域实际上是使用本地 socket 文件来通信）
type  | 套接字类型应为SOCK_STREAM(默认值),SOCK_DGRAM,SOCK_RAW或其他SOCK_常量之一。SOCK_STREAM 是基于TCP的，有保障的（即能保证数据正确传送到对方）面向连接的SOCKET，多用于资料传送。<br />SOCK_DGRAM 是基于UDP的，无保障的面向消息的socket，多用于在网络上发广播信息。
proto  | 协议号通常为零,可以省略,或者在地址族为AF_CAN的情况下,协议应为CAN_RAW或CAN_BCM之一。
fileno  | 如果指定了fileno,则其他参数将被忽略,导致带有指定文件描述符的套接字返回。与socket.fromfd()不同,fileno将返回相同的套接字,而不是重复的。这可能有助于使用socket.close()关闭一个独立的插座。

#### 总结：socket网络编程区分开TCP和UDP就可以了。还有一些网络的知识需要略懂