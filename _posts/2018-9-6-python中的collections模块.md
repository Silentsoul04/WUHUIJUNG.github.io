---
layout: post
title: collections模块
date: 2018-9-6
categories: blog
tags: [python]
description: 文艺的流氓。
---
### collections模块
 - 是一个额外的数据类型扩展模块

#### 有名字的元组
```python
from collections import namedtuple
这里的porint就是一个名字。然后其中有三个键
Porint = namedtuple('porint',['a','b','c'])       这里的名字都可以自己定义
p = Porint(1,2,3)
p1 = Porint(3,2,1)
print(p.a)    结果是：1
print(p.b)    结果是：2
print(p.c)    结果是：3
print(p)      结果是：porint(a=3, b=2, c=1)
print(p1.a)
print(p1.b)
print(p1.c)	  这里的结果同样也是
print(p1)
```

#### 队列queue --> 单向的队列的概念就是说先进去的先出来。不能改变顺序
```python
import queue
q = queue.Queue()
q.put([1,2,3,4,5,6])
q.put("asdas")
print(q.qsize())    输出队列中的长度
print(q.get())
print(q.get())
print(q.get())      如果没有值了，程序不会报错也不会停止运行。会阻塞
```

#### 双向队列 --> deque
```python
from collections import deque
q = deque(['a','b','c'])
q.append('e')           #在队列的结尾添加
q.appendleft('d')       #在队列的头部添加
q.insert(0,3)           #在0下标的地方插入一条数据
q.extend('aaa')         #在最后添加多个数据
q.extendleft('bbb')     #在最前面添加多个数据
print(q)
q.clear()               #清空整个队列
print(q)
print(q.pop())          #从最后的地方取出
print(q.popleft())      #从最开始的地方取出
l = [1,3,4,5,6,1]
d = deque(l)             #把一个列表转换成队列
print(d)
print(d.count(1))        #统计1在队列中出现的次数
```

#### 有序的字典 --> OrderedDict
```python
字典是无序，但是可以使用OrdereDict来进行排序
但是有序字典占用的内存比较多
from collections import OrderedDict
od = OrderedDict([('a',1),('b',2),('c',3)])
print(od)
print(od['a'])
for i in od:
    print(i)

```

#### 默认值的dict --> defaultdict
```python
它的其他功能与dict相同，但会为一个不存在的键提供默认值
其中小于66的值会放到k1中去。而大于66就会放入到k2
from collections import defaultdict
values = [11,22,33,44,55,66,77,88,99]
my_dict = defaultdict(list)           #默认提供了一个列表类型
for value in values:
    if  value>66:
        my_dict['k1'].append(value)
    else:
        my_dict['k2'].append(value)
print(my_dict)

可能会有点不明白，再来看下其他的列子
我这里给了值得一个默认值。是str类型的
import collections
values = ['aa','bb','cc','dd','ee','rr']
cont = collections.defaultdict(str)
for ha in values:
    cont[ha] += 'r'
print(cont)
同样可以自己定义函数
from collections import defaultdict
def demo():
    return '我是最帅的'
dea = defaultdict(demo)
dea['aaa']
print(dea)
返回的值的：defaultdict(<function demo at 0x00B89150>, {'aaa': '我是最帅的'})
```

#### 献上我画的一张图片(鬼画符)
![WUJINLIN](http://i2.bvimg.com/660902/f2bbbbcd31c385b6.png)
#### 总结：这个模块我是以前没有接触过的。而且这几种的数据类型我也是没用过的。其中的defaultdict这个默认值的dict可能会经常用到