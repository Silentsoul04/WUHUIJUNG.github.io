---
layout: post
title: Javascript基础一
date: 2018-12-17
categories: blog
tags: [JavaScript]
description: 文艺的流氓。
---

#### Javascript初探

对于`javascript`这门语言是我自己最近挖洞的时候，发现自己的`Javascript`知识严重的不足，现在是专门来学习这一门语言。

 `Javascript`语言是不能够操作磁盘中的文件的。不过可以通过日志来进行记录。

在`JavaScript`中`script`标签是包裹着`JavaScript`代码的。而浏览器识别到了`script`标签就会自动加载其中的`JavaScript`代码。

而在以前的`html4`标准中`script`标签需要设置一些很没有什么用的属性，比如：`type`而设置的`type`属性通常都是`type="text/javascript"`还有一个就是`languane`属性。这个属性的意思是当前使用的脚本的语言。这肯定是`JavaScript`,所以到了现在的`html5`已经不需要使用这个属性了。

##### JavaScript中的注释符

`JavaScript`中的注释符有两种。第一种就是`//`，第二种`/**/`。就像`PHP`语言一样。

#####  JavaScript引入外部脚本

一般来说我们需要写大量的`javascript`代码,就不会直接放到html代码中，而是会使用一个`js`的脚本来进行存储大量的代码。而我们需要在`html`中引用的时候只需要使用`script`标签就可以把这个文件中的`js`代码引用。而这样的好处就是,使用`script`标签来进行引用了这个`js`文件。浏览器就会下载这个文件一次。等`html2`这个页面使用`script`来进行引用相同的`js`文件的时候,浏览器会直接冲缓冲中加载。这样你的页面会加载的更快。

```javascript
<script src="1.js">alert(2);</script>
```

可以看得到,我这里`script`中使用`src`导入了`1.js`.这个`1.js`是和这个页面同级目录。

其中这里的`alert(2)`是不会加载的。如果`script`标签中的设置了`src`属性，那么被`script`包裹起来的代码将不会工作。必须要从中选择一个。

如果需要运行包裹起来的代码，那么可以在写一个`script`

#### 代码结构

我们已经知道了使用`alert('Hello,world!')`可以来显示消息。可以在代码中编写任意数量的语句，其中使用分号来分割。

来分割`hello,world！`

```javascript
alert('Hello');alert('World!');
```

注意！分号代表着语句的结束。

也可以分两行来进行表示

```javascript
alert('Hello')
alert('World!')
```

换行被称为自动分号插入。在很多的时候有这个特性但是在有的时候是不存在的。

比如下面这个例子:

```javascript
alert(1+
     2+
     3)
```

代码输出的结果是6

我想着应该是运算符都是可以这样的。所以我试试这个例子：

```javascript
alert(1+
     2*
     3
   -3
/2)
```

最后输出的结果是`5.5`,原因就是因为以运算符结尾,那么就是一个`不完整的表达式`.所以不需要分号。

报错的例子：

```javascript
alert("There will be an error")
[1, 2].forEach(alert)
```

运行的时候先会显示`There will be an error`然后就会显示一个出错。在`alert`后面加上一个分号就能够正常运行。以后在写`javascript`还是需要加上分号，一种习惯吧。

这个例子我以为使用函数就能够报错。结果是我使用自定义的函数还是内置函数都是不可行的。

#### 严格模式

直到 2009 年 ECMAScript 5 (ES5) 的出现。ES5 规范增加了新的语言特性并且修改了一些已经存在的特性。为了保证旧的功能能够使用，大部分的修改是默认不生效的。你需要一个特殊的指令——`"use strict"`来明确的使用这些特性。

将`"use strict"`放到脚本文件的最顶部,那么整个脚本文件都是处于严格模式中。同样也可以把`"use strict"`可以放到函数中。则整个函数都是严格模式。

如果需要整个脚本开启严格模式，则需要把`"use strict"`放到脚本的最开始，只能够有注释在`"use strict"`上面，要不然就无法启用严格模式。并且所有的浏览器都支持严格模式。

#### 变量

我们可以通过使用`let`和`var`来创建一个变量。`const`来创建一个常量

先来说说常量吧。声明一个常量的话,一般常量名都是大写。就是全部大写的那种.常量之所以叫常量就是指那种不可变得数据。来看看声明方式。

```javascript
//使用const来进行声明
const NAME = 'wujinlin';
alert(NAME)
```

上述代码将会提示我的名字`wujinlin`.常量的值是无法修改的。

```javascript
//使用const来声明一个常量
const NMAE = 'wujinlin';
//尝试的修改NAME
NAME = 'WUJINLIN';
//这个时候会发生一个错误
```

好了,说完了常量，再来谈谈变量。

声明变量的方式可以使用`var`和`let`来进行声明变量，其中`let`和`var`其中的关系,以后会说到。

```javascript
//开启严格模式
"use static"
//使用let来进行声明变量
let user = 'WUJINLIN';
//var声明变量方式
var name = 'wujinlin';
//弹出两个姓名
alert(user);
alert(name);
```

变量命名的方式建议使用驼峰法来进行命令是比较好的。命令支持`$`,`_`.`and`.也是允许使用中文和象形文字来进行作为变量名字的。但是不推荐这样做。

```javascript
//变量尽量能够让别人看得懂，你这个变量是什么意思。
let My_Windows_Version = '10.12';

alert(My_Windows_Version);
```

声明变量的时候对大小写是敏感的。比如：

```javascript
let Windows = 'Yes';
let windows = 'No';
alert(Windows);
alert(windows);
//输出的结果是完全不同。只有一个字母的大小写不同。
```

也可以变量之间互相`拷贝`

```javascript
//开启严格模式
"use static"
//创建一个变量user
let User = 'wujinlin';
//创建一个变量name
let Name = User;
//输出Name.
document.write(Name);
```

有些变量名是不能够使用的，比如：`return`、`let`、`class`、`function`这些被语言本身采用了。

#### 数据类型

在`JavaScript`中有七种的基本类型:

##### Number

`number`类型,用于任何类型的数字：整数或者浮点数。

```javascript
"use static"
//定义一个值为123的变量num
let num = 123;
//定一个值为123.123的变量num2
let num2 = 123.123;
//查看变量的类型可以使用typeof
alert(typeof num)
alert(typeof(num2))
//可以作为运算符也可以作为函数，也就是有括号和没有括号没区别。
```

除了常规的数字还有一些特殊的数值。也是属于这个类型：`Infinity`、`-Infinity`和`NaN`

- `Infinity`代表数学概念中的无穷大。比任何一个数字都大的特殊值。可以通过除以`0`来得到

```javascript
//得到Infinity
alert( 1 / 0 );
```

- `NaN`代表一个计算错误。

```javascript
//当number计算的时候发现的一个错误 就会用NaN来显示，比如字符串和数字相除
alert("I'm is good" / 1);
//NAN是粘性的。
alert('Im' / 1 + 2);
```

`javascript`中做数学运算是安全的。因为脚本不会死亡,最坏的情况下会得到`NaN`.

##### String类型(字符串类型)

接下来就是字符类型`String`

`javascript`中的字符串必须被包含在引号里面。

在`javascript`中有三种包含字符串的方式。

1. 单引号：`'Hello'`
2. 双引号：`"Hello"`
3. 反引号：``Hello``

反引号是功能扩展的引用,允许通过`${..}`,将变量和表达式嵌入到字符串中。

```javascript
let str = 'Hello';
let str1 = "World";
// 合并两个
let str2 = `${str},${str1}`;
```

反引号中还可以放入其他的表达式:

```javascript
alert(`This year is ${1 + 2}`)
```

这样的格式,只能在反引号中有用。其他的引号并不会有这样的效果。

`JavaScript`语言中的只有`string`类型，可以代表一个或者多个字符。

##### Booblean类型(逻辑类型)

`Booblean`类型就包含了两个值：`True`和`False`.

这个类型很多时候用户两个值比较之后的结果：

```javascript
let num1 = 4 < 2;
alert(num1);
//返回的结果就是false
```

##### Null值

特殊的`null`值不属于上述的任何一种类型。

```javascript
let nu = null;
```

`javascript`中的`null`就是一个没有任何意义的值。并不是某些语言中的对'对不存在对象的引用'或者是`null`指针

##### Undefined值

和特殊值`null`一样,自成一个类型。`undefined`的含义就是`没有赋值的变量`。

```javascript
//就像这样
let xxx;
//上面定义了xxx，但是下面什么都没有。
alert(xxx);
```

`undefined`值,常常被用来检查变量是否赋值。

##### 还有两个类型，object类型和symbol类型。不过这两个类型都是对象操作的。等到对象的时候将会作出解释。

总结：学了`javascript`的五个小节还是基础的。感觉和`php`这类的语言挺像的。这五小节。前四小节没有什么可说的。在数据类型方面。官方说了有一处错误。使用`typeof null`的时候显示的数据类型是`object`类型。因为`null`是自成一种类型。所以当遇到这个问题的时候。就能够解释了。后面学习对象的时候会自然而然的学习到`object`类型和`symnol`类型。





