---
layout: post
title: 正则和re模块
date: 2018-9-5
categories: blog
tags: [python]
description: 文艺的流氓。
---
#### 正则表达式
```
字符组：在同一个位置可能出现的各种字符组成了一个字符组，在正则表达式中用[]表示
字符分为很多类，比如数字，字母，标点。
[0-9] 就是匹配0到9中的任意数字
[a-z] 就是匹配小写字母a到小写字母z
[A-Z] 就是匹配大写字母A到大写字母Z
这三种也可以随意组合
```
#### 元字符
```
  .     匹配除换行符以外的任意字符
  \w    匹配字母或数字或下划线
  \s    匹配任意的空白符
  \d    匹配数字
  \n    匹配一个换行符
  \t    匹配一个制表符
  \b    匹配一个单词的结尾         .\b是匹配任意单词的结尾
  ^     匹配字符串开始             startswith()相当于
  $     匹配字符串的结尾
  \W    匹配非字母或数字或下划线
  \D    匹配非数字
  \S    匹配非空白符
  a|b   匹配字符a或字符b
  ()    匹配括号内的表达式，也表示一个组
  [...] 匹配字符组中的字符
  [^...]匹配除了字符组中字符的所有字符
```
#### 全局匹配
```
\W\w 能够匹配任意
\D\d 能够匹配任意
\S\s 能够匹配任意
```
#### 量词
```
  *       重复零次或更多次     贪婪匹配
  +       重复一次或更多次     贪婪匹配
  ?       重复零次或一次       贪婪匹配
其实+和?都是从*中脱离出来的
  {n}     重复n次
  {n,}    重复n次或更多次
  {n,m}   重复n到m次
```
#### 贪婪模式和非贪婪模式
```
贪婪匹配的过程就是，直接会先匹配全部所有的。然后需要其他的，就会往会找。这叫做回溯算法
非贪婪匹配(惰性匹配),会一个一个的匹配如果在前面找到了一个就会直接结束匹配。不会管后面的
```
#### 一些其他的用法
```
.*?x 的意思就是只要匹配到了一个x就会结束匹配

进行或匹配的时候。需要把长的放前面。
如果需要匹配转义符号。前面加上r比如：r'\d'

其中?在正则表达式中是挺重要的一个
在量词中是匹配一次或零次。在表达式最后是表示惰性匹配。如果放在分组的第一个。就是取消分组空间
```
#### re模块 是用于正则匹配的一个模块，介绍一些方法
#### findall 返回所有满足的条件，放在列表里面
```python
import re
ret = re.findall('a','eva egon yuan')
print(ret)
结果：['a', 'a']
```
#### search 返回的结果是一个对象。需要使用group()来进行打印
```python
import re
ret = re.search('a','eva egon yuan') 从前往后找到一个就返回
print(ret.group())        使用group来进行打印
print(ret)
如果没有找到。调用group会报错
```
#### match 是从头开始匹配的 如果正则规则从头开始可以匹配上 就返回一个变量
```python
import re
ret = re.match('e','eva egon yuan')
print(ret.group())          同样是使用group调用
print(ret)
```
#### split 分割
```python
先按'a'分割得到''和'bcd'在对''和'bcd'和'b'分割得到 ['', '', 'cd']
import re
ret = re.split('[ab]','abcd')
print(ret)
返回的结果：['', '', 'cd']
```
#### sub替换
```python
将数字替换成J
import re
ret = re.sub('\d','J','asd123sds3432asdas')
print(ret)
返回结果：asdJJJsdsJJJJasdas
```
#### subn替换
```python
返回一个元组，并且会告诉我们一共替换了多少次
import re
ret = re.subn('\d','J','asd123sds3432asdas')
print(ret)
返回结果：('asdJJJsdsJJJJasdas', 7)
```
#### compile 把一条正则编译成Pattern对象。哪么需要使用到这条正则的时候就可以直接使用
```python
使用了search来进行匹配。返回的结果需要使用group来进行输出
import re
obj = re.compile('\d{3}')
ret = obj.search('sadasda123asdas')
print(ret.group())
```
#### finditer 拿到的结果是一个迭代器
```python
使用迭代器的方法拿出来结果来
import re
ret = re.finditer('\d','asdas123asdas23123dasd')      匹配字符串中的数字
for i in ret:
    print(i.group())              通过for循环出来的也不是直接值。也需要使用group打印出来
```
#### 使用search可以
```python
import re
ret = re.search('^[1-9](\d{14})(\d{2}[0-9x])?$','124214789654356824')
print(ret.group())      # 返回的结果：124214789654356824
print(ret.group(1))     # 返回的结果：24214789654356
print(ret.group(2))     # 返回的结果：824
```
#### 使用match同样是可以的
```python
import re
ret = re.match('(^[a-z]{5})(\d{3})','asdas123asdas23123dasd')
print(ret.group())
print(ret.group(1))
print(ret.group(2))
```
#### 取消优先级
```python
import re
其中的?:就是取消分组优先
ret = re.findall('www.(?:baidu|whj).(?:com|website)','www.whj.website')
print(ret)
输出：['www.whj.website']
ret = re.findall('www.(?:baidu|whj).(?:com|website)','www.whj.website')
print(ret)
输出：['whj','website']
```
#### split中的优先级
```python
如果在分割中有分组，那么将会保留分割的部分
import re
ret=re.split("(\d+)","eva3egon4yuan")
print(ret)
结果 ： ['eva', '3', 'egon', '4', 'yuan']
```
#### flags
```python
import re
ret = re.findall('\d','12asdsadas123    sasd',re.S)
print(ret)
flags有很多可选值：
re.I(IGNORECASE)忽略大小写，括号内是完整的写法
re.M(MULTILINE)多行模式，改变^和$的行为
re.S(DOTALL)点可以匹配任意字符，包括换行符
re.L(LOCALE)做本地化识别的匹配，表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境，不推荐使用
re.U(UNICODE) 使用\w \W \s \S \d \D使用取决于unicode定义的字符属性。在python3中默认使用该flag
re.X(VERBOSE)冗长模式，该模式下pattern字符串可以是多行的，忽略空白字符，并可以添加注释
```
#### 匹配标签
```python
还可以在分组中利用?<name>的形式给分组起名字
通过给分组名字就可以使用group获取
import re
ret = re.search(":(?P<aaa>\w+):\w+:(?P=aaa):",":bbb:aaa:bbb:")
print(ret.group())
print(ret.group('aaa'))
```
#### 普通的做法
```python
import re
ret = re.search(":(\w+):\w+:(\w+):",":bbb:aaa:bbb:")
print(ret.group())
print(ret.group(1))
```
#### 匹配整数
```python
其中也是有小数的。使用优先级把小数替换成了空
import re
ret = re.findall(r"\d+\.\d+|(\d+)","1-2*(60+(-40.35/5)-(-4*3)+64.23)")
ret.remove("")        一次也只能够去除一个空格
ret.remove("")
print(ret)
```
#### 总结：这些只是一些基本的正则。让我了解正则匹配的魔力。自己还写一个脚本。是爬虫的。虽然是参考的别人的。但是自己也是理解了
#### 爬虫源码
```python
import re
import urllib.request

获取网页源代码
def gethtml(url):
    get = urllib.request.urlopen(url)
    html = get.read()
    return html
填入需要爬取的网页.
html = gethtml('https://images.o2oxy.cn/')
把网页源代码就进行UTF-8编码
html = html.decode('UTF-8')
正则匹配爬取网页中的图片。也就是把正则匹配源代码
def getjpg(html):
    reg = r'src="([.*\S]*\.jpg)"'
    image = re.compile(reg)
    imagelist = re.findall(image,html)
    return imagelist
imagelist = getjpg(html)
imgName = 0
把爬取到的图片放入文件中。以二进制的形式
利用for循环一个一个的写
for imagePath in imagelist:
    try:
        f = open('D:\PyChamwenjian\图片\\'+str(imgName)+".jpg",'wb')
        f.write((urllib.request.urlopen(imagePath)).read())
        print(imagePath)
        f.close()
    except Exception as e:
        print(imagePath+"error")
    imgName +=1
    print("ALL Done")
```
#### 这个爬虫中的正则其实很简单的。就是找出网页中的src后面的图片
#### 最后还有我自己画的一张图片
![WUJINLIN](https://s1.ax1x.com/2018/09/15/iEv5Ux.png)









