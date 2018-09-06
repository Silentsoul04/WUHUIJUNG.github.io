---
layout: post
title: random模块
date: 2018-9-6
categories: blog
tags: [python]
description: 文艺的流氓。
---
### random是一个随机的模块
#### 随机小数
```python
import random
print(random.random())      # 只会随机0到1中的数
print(random.uniform(1,3))  # 随机大于1小于3的数
```
#### 随机整数
```python
print(random.randint(1,5))  # 随机1到5中的数包括五
print(random.randrange(0,10,2)) # 随机0到10中的整数(不包括10)
print(random.randrange(1,10,2)) # 随机1到10中的奇数
```
#### 随机选择一个
```python
print(random.choice([1,'aa',[222,555]]))    # 从中随机选择一个需要注意的是格式是([])
```
#### 随机返回多个
```python
print(random.sample([4,'b',['a'],"我是"],2)) # 随机返回两个后面可以设置返回几个
```
#### 顺序打乱
```python
a = [1,2,3,4,5,6]
print(random.shuffle(a))
print(a)
结果:[2, 1, 6, 3, 5, 4]
```
#### 来看一个小列子：随机生成一个验证码
```python
import random
def demo():
    code = ''
    for i in range(1,5):
        num = random.randint(0,9)
        bbb = chr(random.randint(65,90))
        add = random.choice([num,bbb])
        code = "".join([code,str(add)])
    return code
print(demo())
每一次把数字和字母放到code并且给他str一下.
这里的for就是来控制多少个字符
```

#### 总结：随机模块我自我感觉作用挺大的。因为有的时候sh1llc0de会弄一些题目来做。有的时候做逆向题目的时候也需要用到random
