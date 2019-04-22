---
layout: post
title: Python and SSTI
date: 2019-4-21
categories: blog
tags: [WEB]
description: 文艺的流氓。
---

#### 概述

模板引擎可以让（网站）程序实现界面与数据分离，业务代码与逻辑代码的分离，这大大提升了开发效率，良好的设计也使得代码重用变得更加容易。与此同时，它也扩展了黑客的攻击面。除了常规的 XSS 外，注入到模板中的代码还有可能引发 RCE（远程代码执行）。通常来说，这类问题会在博客，CMS，wiki 中产生。虽然模板引擎会提供沙箱机制，攻击者依然有许多手段绕过它

转载至:[这里](https://strcpy.me/index.php/archives/761/)

#### 一个Python的例子

这几年比较火的一个漏洞就是`jinjia2`之类的模板引擎的注入，通过注入模板引擎的一些特定的指令格式，比如`{{1+1}}`而返回了2得知漏洞存在。实际类似的问题在 `Python` 原生字符串中就存在，尤其是`Python 3.6`新增 f 字符串后，虽然利用还不明确，但是应该引起注意。

```python
userdata = {"user":"whj","password":"whj"}
passwd = raw_input("password:")

if passwd != userdata["password"]:
    print "Password" + passwd + "is wrong for user %(user)s" % userdata
```

这个例子如果用户是输入的`%(password)s`那么就可以获取到用户的真实密码。

我个人觉得是在进行比较的时候把输入的转换成了`Python`代码执行

#### format相关的一些方法

可以先查看下这个[官方文档](https://docs.python.org/3/library/functions.html#format)

换成这个也是可以的

```python
userdata = {"user":"whj","password":"whj"}
passwd = raw_input("password:")

if passwd != userdata["password"]:
    print ("Password " + passwd + " is wrong for user {user}").format(**userdata)
```

同样的Payload也是可以执行的

`format`是先替换然后在获取

```python
import os

print('{1.system}'.format(os,os))
```

结果是`<built-in function system>`

但是貌似只能获取属性，不能执行方法？但是也可以获取一些敏感信息了。

例子: http://lucumr.pocoo.org/2016/12/29/careful-with-str-format/

```python
CONFIG = {
    'SECRET_KEY': 'super secret key'
}

class Event(object):
    def __init__(self, id, level, message):
        self.id = id
        self.level = level
        self.message = message

def format_event(format_string, event):
    return format_string.format(event=event)
```

如果 `format_string `为 `{event.__init__.__globals__[CONFIG][SECRET_KEY]} `就可以泄露敏感信息。

理论上，可以通过类的各种继承关系找到想要的信息，比如在 Django 中的思路为 https://xianzhi.aliyun.com/forum/read/615.html

#### Python3.6中的`f`字符串

这个字符串非常厉害，和 Javascript ES6 中的模板字符串类似，有了获取当前 `context` 下变量的能力。

https://docs.python.org/3/reference/lexical_analysis.html#f-strings

```python
a = "Hello"
b = "f{a} World"
print(b)
```

而且不仅仅限制为属性了，代码可以执行了。

但是貌似没有把一个普通字符串转换为 `f` 字符串的方法，也就是说用户很可能无法控制一个 `f` 字符串，可能无法利用，还需要继续查一下。

```python
import os
print(f"{os.system('ls')}")
```

在能够出现xss的地方就可能存在这个模板注入漏洞,不是唯一的。有可能是过滤了xss但是其他的符号没有过滤。细心就能够挖到的。















