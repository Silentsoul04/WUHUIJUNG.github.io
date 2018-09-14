---
layout: post
title: time模块
date: 2018-9-6
categories: blog
tags: [python]
description: 文艺的流氓。
---
### time模块 是一个和时间有关系的模块
#### 常用的方法
```python
import time
time.sleep(3)       # 停止时间3秒。以秒为单位
time.time()         # 获取当前时间戳
```
#### 表达时间的三种方式
- 时间戳
- 格式化时间字符串
- 元组形式

#### 时间戳 -- 其实时间戳很简单 Timestamp
```python
时间戳时从1970年的1月1号开始计算的
import time
print(time.time())        # 获取当前时间戳
print(type(time.time()))  # 是<class 'float'>
```
#### 格式化时间字符串	Format string
```python
import time
print(time.strftime('%B %A'))       # 显示完整的月份和星期
print(time.strftime('%b %a'))       # 显示缩写的月份和星期
print(time.strftime('%Y/%m/%d %H:%M:%S'))   # 显示完整的当前时间,包括时分秒
print(time.strftime('%y/%m/%d %I:%M:%S'))   # 显示简写的年份和完整的月日。包括时分秒
print(time.strftime('%%'))                  # 就是显示%%
print("今天是:",time.strftime('%j'),"天")   # 显示今天是一年中的那一天
print(time.strftime('%c'))                  # 和本地相应的日期
print(time.strftime('%U'))                  # 显示一年中先现在是那个星期
print(time.strftime('%x'))                  # 显示本地的日期
print(time.strftime('%X'))                  # 显示当前的时间
```
#### python中时间日期格式化符号
```
%y 两位数的年份表示（00-99）
%Y 四位数的年份表示（000-9999）
%m 月份（01-12）
%d 月内中的一天（0-31）
%H 24小时制小时数（0-23）
%I 12小时制小时数（01-12）
%M 分钟数（00=59）
%S 秒（00-59）
%a 本地简化星期名称
%A 本地完整星期名称
%b 本地简化的月份名称
%B 本地完整的月份名称
%c 本地相应的日期表示和时间表示
%j 年内的一天（001-366）
%p 本地A.M.或P.M.的等价符
%U 一年中的星期数（00-53）星期天为星期的开始
%w 星期（0-6），星期天为星期的开始
%W 一年中的星期数（00-53）星期一为星期的开始
%x 本地相应的日期表示
%X 本地相应的时间表示
%Z 当前时区的名称
%% %号本身
```
#### 元组形式的时间：struct_time一共有九个元素
```python
import time
print(time.localtime())     # 返回给定时间戳对应的本地时间
print(time.gmtime())        # 返回UTC本地时间
也可以获取元组中的任意元素
tim = time.localtime()
print(tim.tm_year)           # 返回年
print(tim.tm_hour)           # 返回时
print(tim.tm_isdst)          # 是否夏令时
print(tim.tm_mday)           # 返回日
print(tim.tm_min)            # 返回分
print(tim.tm_mon)            # 返回月
print(tim.tm_sec)            # 返回秒
print(tim.tm_wday)           # 返回周几。(0表示周一)
print(tim.tm_yday)           # 返回一年中的第几天
差不多这几个常用的
```
#### 全部的方法

| 索引| 属性|  值  |
| --------| -----:  | :----:|
| 0 | tm_year(年)| 2018 |
| 1 | tm_mon(月) | 1 - 12|
| 2 | tm_mday(日)|  1 - 31  |
| 3 | tm_hour(时)|  0 - 23  |
| 4 | tm_min(分) |  0 - 59  |
| 5 | tm_sec(秒) |  0 - 60  |
| 6 | tm_wday(weekday)|  0-6(0表示周一)  |
| 7 | tm_yday(一年中的第几天)|  1-366  |
| 8 | tm_isdst(是否是夏令时) |  默认为0  |

#### 还有其他的用处
```python
import time
t1 = time.clock()       # 返回当前CPU的时间
```
#### 小结：时间戳是计算机能够识别的时间；时间字符串是人能够看懂的时间；元组则是用来操作时间的

#### 接下来是三种格式中的转换:如果时间戳需要转换成字符串。那么需要通过结构化来进行转换
![WUJINLIN](https://s1.ax1x.com/2018/09/15/iEvmge.png)
#### 时间戳-->结构化时间
```python
import time
print(time.gmtime(2000000000))        # 通过gmtime来进行转换
print(time.localtime(2000000000))     # 通过localtime来进行转换
```
#### 结构化时间-->时间戳
```python
import time
time_tuple = time.localtime()         # 获取当前的时间戳
print(time.mktime(time_tuple))        # 通过mktime来进行转换
```
#### 结构化时间-->字符串时间
```python
import time
print(time.strftime('%Y/%m/%d',time.localtime(2000000000)))
```
#### 字符串时间-->结构化时间
```python
import time
print(time.strptime('2018/9/06','%Y/%m/%d'))
其中需要注意的就是结构化和字符串转换的时候需要注意。需要声明转换的结构
```
#### 结构化时间--> %a %b %d %H:%M:%S %Y串 和 时间戳的转换
```python
import time
print(time.asctime(time.localtime()))     # 结构化时间
print(time.ctime(2000000000))             # 时间戳
```
#### 计算时间. 把时间转换成了时间戳.然后再进行相减.最后又转换回去
```python
import time
t1 = time.mktime(time.strptime('2018-09-06 08:30:1','%Y-%m-%d %H:%M:%S'))
t2 = time.mktime(time.strptime('2018-09-06 15:30:1','%Y-%m-%d %H:%M:%S'))
t3 = t2 - t1
t4 = time.gmtime(t3)
print("过去了%d小时" % t4.tm_hour)
格式化字符串把小时弄过来了
```
#### 总结：之前还是不知道time模块有这么多的方法。以前就是使用过了time.sleep


