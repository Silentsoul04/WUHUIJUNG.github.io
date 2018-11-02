---
layout: post
title: python对ftp扫描
date: 2018-11-1
categories: blog
tags: [python]
description: 文艺的流氓。
---

#### 刚刚写完了ftp的架设。想试试利用python来进行扫描ftp弱口令

需要使用到ftplib模块(我是使用的python3)

一段简单的代码：

```python
import ftplib

def anony(hostname):
    ftp = ftplib.FTP(hostname)
    ftp.login('anonymous','anonymous')
    print('\n[*]'+str(hostname)+ ':FTP is login')
    ftp.quit()

hostname = '192.168.43.90'
anony(hostname)
```

一个多个IP的实现。其实就是加了一个`for`循环

```python
from ftplib import FTP
import socket

socket.setdefaulttimeout(1)

def anony(hostname):
    for i in range(254):
        hostname_tmp = hostname + str(i)
        try:
            ftp = FTP(hostname_tmp)
            ftp.login('anonymous', 'anonymous')
            print(ftp.getwelcome())  # 输出欢迎信息
            print('\n[*]' + str(hostname_tmp) + ':FTP is login')
            ftp.close()
        except:
            print('\n[-]' + str(hostname_tmp) + ':FTP is failed')
            continue
hostname = '192.168.43.'
anony(hostname)

```

一个空格让我修了bug这么久😢

上代码

```python
from ftplib import FTP
import socket


socket.setdefaulttimeout(1)         # 设置了一个超时时间

def anony(hostname):
    for i in range(200,254):
        hostname_tmp = hostname + str(i)
        try:
            ftp = FTP(hostname_tmp)
            ftp.login('anonymous', 'anonymous')
            print(ftp.getwelcome())  # 输出欢迎信息
            print('\n[*]' + str(hostname_tmp) + ':FTP is login')
            return ftp
        except:
            print('\n[-]' + str(hostname_tmp) + ':FTP is error†')
            continue
def Downloadfile(ftp,filename):
    bufsize = 1024
    file_hadle = open('123.txt','wb')
    ftp.retrbinary('RETR '+filename,file_hadle.write, bufsize)
    ftp.close()
    print('下载成功！')
def Document():
    with open('123.txt','r') as f:
        print('文件内容:'+f.read())
if __name__ == '__main__':
    hostname = '192.168.8.'
    ftp = anony(hostname)
    filename = ftp.nlst()
    print(filename)
    file_name = input('需要下载那个文件：')
    Downloadfile(ftp,file_name)
    Document()

```

总的来说就是一个`ftplib`的使用











