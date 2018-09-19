---
layout: post
title: OS模块
date: 2018-9-7
categories: blog
tags: [python]
description: 文艺的流氓。
---
### OS模块 --> 是一个与操作系统交互的接口

#### 系统命令
```python
import os
print(os.system('ipconfig'))      直接运行系统命令，没有返回值。会直接输出
a = os.popen('dir').read()        运行系统命令，并且是有返回值的.并且编码会是UTF-8
print(a)
print(os.getcwd())                获取当前工作目录相当于Linux中的pwd
print(os.chdir('aaa'))            改变目录，相当于Linux中的cd
```
#### 判断操作系统
```python
print(os.sep)             输出操作系统特定的路径分隔符，win下为"\\",Linux下为"/"
print(os.name)            输出当前平台使用的行终止符，win下为"\r\n",Linux下为"\n"
print(os.linesep)         输出用于分割文件路径的字符串 win下为;,Linux下为:
print(os.pathsep)         输出字符串指示当前使用平台。win->'nt'; Linux->'posix'
```
#### 目录文件相关的
```python
os.makedirs('aa/aa2')     创建两个目录。一个父目录aa一个子目录aa2
os.removedirs('aa/aa2')   若目录为空就删除。删除的方法就是先探测本目录是否为空，如果为空就查看上级目录是否为空。如果目录为空就是删除
os.mkdir('aaa')           生成单级目录
os.rmdir('aaa')           生成单级目录
os.listdir('aaa')         列出指定目录下的文件和子目录，包括隐藏文件
os.rename('aaa.txt','bbb.txt') 文件重命名
os.remove('bbb.txt')      删除文件
print(os.stat(os.getcwd())) 获取文件目录的信息
```
#### 路径相关的
```python
print(os.path.abspath(r'D:\PyChamwenjian\venv'))      # 返回path规范化的绝对路径
print(os.path.split(os.getcwd()))                     # 将path分割成目录和文件名二元组返回
print(os.path.dirname(os.getcwd()))                   # 返回path的目录。其实就是os.path.split(path)的第一个元素
print(os.path.basename(os.getcwd()))                  # 返回os.path.split(path)的第二个元素
print(os.path.exists(os.getcwd()))                    # 如果path存在，返回True；如果path不存在，返回False
print(os.path.isabs(os.getcwd()))                     # 如果path是绝对路径，返回True
print(os.path.isfile(os.getcwd()))                    # 如果path是一个存在的文件，返回True。否则返回False
print(os.path.isdir(os.getcwd()))                     # 如果path是一个存在的目录，则返回True。否则返回False
print(os.path.getatime(os.getcwd()))                  # 返回path所指向的文件或者目录的最后访问时间
print(os.path.getmtime(os.getcwd()))                  # 返回path所指向的文件或者目录的最后修改时间
print(os.path.getsize(os.getcwd()))                   # 返回path的大小
print(os.path.join(filepath,filename)) 								  # 是用来拼接路径的 
```

#### 还有一张我自己做的分类图
![WUJINLIN](https://s1.ax1x.com/2018/09/15/iEv4V1.png)

### 总结：OS模块主要是对文件的操作。系统命令的使用，还能够判断系统是什么。OS模块对跨平台的作用挺大的




