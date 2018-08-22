---
layout: post
title: IO操作
date: 2018-8-21
categories: blog
tags: [python]
description: 文艺的流氓。
---
#### 只读
```python
a = open('aaa','r',encoding='UTF-8')     #一个简单的读取文件
b = a.read()                             #直接访问当前路径下的文件
print(b)                                 #遇到了一个问题，就是编码的问题.
a.close()
# 读取文件的时候，需要设置为UTF-8的格式，否则的话会是以GBK的格式去读取字符串
a = open('aaa','rb')                     #rb的意思就是说只读
b = a.read()                             #用于那种不是文本类型的文件，比如图片
print(b)
a.close()
结果：b'\xe9\x98\xbf\xe6\x96\xaf\xe9\xa1\xbf\xe6\x92\x92\xe6\x97\xa6\xe6\x92\x92\xe6\x97\xa6'
```
#### 读写
```python
a = open('aaa','r+',encoding='UTF-8')     #读写，只能够先读然后在写
print(a.read())
a.write('你是是')
a.close()

a = open('aaa','r+',encoding='UTF-8')
a.write('aaa')                            #如果是先写的话，那么此时的光标在最开始的位置
print(a.read())                           #只会返回光标后面的值
a.close()

```
#### 只写
```python
a = open('bbb','w',encoding='UTF-8')  #这就是简单的写，如果写入的文件不存在那么是会创建
a.write('我是最帅的')                  #如果是文件存在，那么会把以前的内容覆盖掉
a.close()

a = open('ccc','wb')                  #以二进制的格式写入文件
a.write('我是最帅的'.encode('UTF-8'))  #需要自己声明编码，否则就只能写入字节类型
a.close()

a = open('bbb','w+',encoding='UTF-8')
a.write('真的爱你')                     #会把文件里面的东西覆盖掉。
a.seek(0)                              #是设置光标
print(a.read())                        #然后读取的话时候光标后面的值
a.close()                              #写完了之后光标的值是在最后哪里
```
#### 追加
```python
a = open('aaa','a',encoding='UTF-8')   #追加，不过指针是在文件的结尾
a.write('wujinlin')
a.close()
```
#### 以二进制打开一个文件追加
```python
a = open('aaa',mode='ab',)              #指针指向结尾
a.write('wujinlinasddasd'.encode('UTF-8'))
a.close()

a = open('aaa','a+',encoding='UTF-8')
a.write('啦啦啦')                    #如果不设置光标位置的话是读取不出来的
a.seek(0)
print(a.read())
a.close()
```
#### seek是设置光标的地址
#### tell是实时告诉光标的位置
```python
a = open('aaa','a',encoding='UTF-8')
a.write('我去')
b = a.seek(0)
print(a.tell(b))
a.close()
```
#### 以行读文件
```python
a = open('aaa','r',encoding='UTF-8')  #文件句柄
print(a.readline())                   #读取文件的一行。也可以指定读取多少
print(a.readlines())                  #读取文件所有行，返回是一个列表
['我去我去\n', 'sadsadsad\n', 'asdsadsadsa']
```
#### 截断文件。
```python
a = open('aaa','a',encoding='UTF-8')
a.truncate(3)                         #保留三个字符，也就是一个中文。
```
#### with打开文件。能够防止忘记了关闭文件
```python
with open('aaa','r',encoding='UTF-8') as f:
    print(f.read())
```
#### 还有一些flush的作用就是把缓存内容强制的写入磁盘
```python
a = open('aaa','a',encoding='UTF-8')
a.write('2112321')
a.flush()                     #将下面的缓存写入了文件中
input('asdasda')

```
<<<<<<< HEAD
#### 这是一个简单的注册验证。
```python
user = input('注册的名字：')
passwd = input('注册的密码：')
with open('aaa','w',encoding='UTF-8') as a:
    a.write('{}\n{}'.format(user,passwd))
print('--------恭喜你注册成功-----------')
c = []
p = 0
while p < 3:
    username = input('请输出你的账号')
    password = input('请输出你的密码')
    with open('aaa','r',encoding='UTF-8') as f:
            for i in f:
                c.append(i)
            if  username == c[0].strip() and password==c[1].strip():
                print('登陆成功')
                print('恭喜恭喜你呀呀呀')
                break
            else:
                print('登陆失败')
```
#### 还有一些利用其他的库来进行对文件的操作
```python
with open('aaa',encoding='UTF-8') as a,open('bbb','w',encoding='UTF-8') as b:
    for line in a:                #遍历出a中的数据
        if  '星儿' in line:       #如果存在的话，那么就是把星儿替换成阿娇
            line = line.replace('星儿','阿娇')
        b.write(line)             #然后再写入bbb文件中
# 使用os库
import os
os.remove('aaa')        #利用os库来进行对文件的删除
os.rename('bbb','你才') #利用os库来进行对文件的重命名
```
=======
#### with open('aaa',encoding='UTF-8') as f: 默认是读。
>>>>>>> d9fc07f9d7f2b5182f244732662c4860a4d1b486
### 还有一些的其他的操作，只能自己去摸索了
## 如果当自己的能力不能够实现自己的野心的时候，也许这时候该去拼命了😔😔😔😔😔😔😔
