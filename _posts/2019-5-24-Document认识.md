---
layout: post
title: Document
date: 2018-5-24
categories: blog
tags: [JavaScript]
description: 文艺的流氓。
---

#### Document文档

简称"DOM",在html中 能够使用dom对象来对html 进行操作

#### 开始

在JavaScript中能够使用这些dom对象来访问这些

`<html>`=`document.documentElement`

`<body>`=`document.body`

`<head>`=`document.head`

有些时候`document.body`的值可能是null

```html
<html>

<head>
  <script>
    alert(document.body)  // null
  </script>
</head>

<body>
  <script>
    alert(document.body) // [object HTMLBodyElement]
  </script>
</body>

</html>
```

第一个body没有值 是因为在运行到JavaScript代码的时候 之前并没有出现Body这个标签 所以才会是Null

访问body所有子元素`childNodes`

```html
<html>

<head>
</head>

<body>
  <script>
    alert(document.body.childNodes)
  </script>
  <P>
    ASD
  </P>
  <h1>
    code!
  </h1>
</body>

</html>
```

结果是都是会输出在页面上的 也可以拿一个变量来代替`document.body` 比如这样

```html
<html>

<head>
</head>

<body>
  <script>
    let elm = document.body;
    elm.childNodes;
  </script>
  <P>
    ASD
  </P>
  <h1>
    code!
  </h1>
</body>

</html>
```

结果是一样的 

`firstChild`和`lastChild`属性是访问第一个和最后一个子元素的快捷方式

```html
<html>

<head>
</head>

<body>
  <script>
    let elm = document.body;
    alert(elm.childNodes[0] == elm.firstChild);
    alert(elm.childNodes[elm.childNodes.length - 1] == elm.lastChild);
  </script>
  <P>
    ASD
  </P>
  <h4>
    I like code!
  </h4>
  <h1>
    code!
  </h1>
  <h2>
    I don't like code!
  </h2>
</body>

</html>
```

都是`true`

dom是一个集合 并不是一个数组  并且集合是一个只读  那么意思就是说不能直接通过修改 dom也是实时的 修改了 立马就会出现

`alert(document.body.childNodes.filter)`

返回的是空。

尽量使用`for...of..`来进行遍历集合 因为集合是可迭代的 不要使用`for....in..`来进行循环集合 因为集合中很少拥有属性

#### 兄弟节点和父节点

兄弟节点就是在同一个父节点下的节点。最常见的就是`body`和`head`就是兄弟节点, 同在`html`下面、

父节点通过`parentNode`

兄弟节点通过`previousSibling`访问

而侄子节点就是通过`nextSibling`

```html
<html><head></head><body><script>  
    // <body> 的父节点是 <html>
    alert( document.body.parentNode === document.documentElement ); // true
  
    // <head> 的下一个兄弟节点是  <body>
    alert( document.head.nextSibling ); // HTMLBodyElement
  
    // <body> 的上一个兄弟节点是  <head>
    alert( document.body.previousSibling ); // HTMLHeadElement
  </script></body></html>
```

#### 获取只有元素的节点

在之前的加上一个`element`

- children` —— 只获取类型为元素节点的子节点。

- `firstElementChild`，`lastElementChild` —— 第一个和最后一个子元素。

- `previousElementSibling`，`nextElementSibling` —— 兄弟元素。
- `parentElement` —— 父元素。

只显示元素`childNod`替换成`children`

```html
<html>

<body>
  <div>Begin</div>

  <ul>
    <li>Information</li>
  </ul>

  <div>End</div>

  <script>
    for (let elem of document.body.children) {
      alert(elem); // DIV, UL, DIV, SCRIPT
    }
  </script>
  ...
</body>

</html>
```

#### 表

到现在为止我们描述了基本的导航属性。

为了方便起见，某些类型的 DOM 元素会提供特定于其类型的额外属性。

Tables 是其中一个很好也是很重要的例子。

<table> 元素支持 (除了上面给出的之外) 以下这些属性:

- `table.rows` — 用于表示表中 `<tr>` 元素的集合。
- `table.caption/tHead/tFoot` — 用于访问元素 `<caption>`、`<thead>`、`<tfoot>`。
- `table.tBodies` — `<tbody>` 元素的集合（根据标准该元素数量可以很多）。

**<thead>、<tfoot>、<tbody>** 元素提供了 `rows` 属性：

- `tbody.rows` — 表内部 `<tr>` 元素的集合。

**<tr>：**

- `tr.cells` — 在给定 `<tr>` 元素下 `<td>` 和 `<th>` 单元格的集合。
- `tr.sectionRowIndex` — 在封闭的 `<thead>/<tbody>` 中 `<tr>` 的编号。
- `tr.rowIndex` — 在表中 `<tr>` 元素的编号。

**<td> 和 <th>：**

- `td.cellIndex` — 在封闭的 `<tr>` 中单元格的编号。

```html
  <table id="table">
    <tr>
      <td>one</td><td>two</td>
    </tr>
    <tr>
      <td>three</td><td>four</td>
    </tr>
  </table>

  <script>
    alert(table.rows[0].cells[1].innerHTML)
  </script>
```

#### 总结

一个dom节点 可以使用导航来访问节点, 差不多主要是讲了节点之间的关系,节点之间的访问。

所有节点使用的方法：`parentNode`、`childNodes`、`firstChild`、`lastChild`、`previousSibling` 和 `nextSibling`。

对于元素节点方法: `parentElement`,`children`,`firstElementChild`,`lastElementChild`,

`previousElementSibling`和`nextElementSibling`.



