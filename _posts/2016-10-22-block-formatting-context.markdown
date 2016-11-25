---
layout:     post
title:      "理解块级格式上下文"
date:       2016-10-22
author:     "Yu"
header-mask: 0.6
catalog: true
tags:
    - 前端布局
---

块级格式上下文（Block Formatting Context）是CSS中一个相对冷门的概念，以至于很多从事前端开发多年的程序员对它也不是很熟悉。网上有很多关于BFC的文章，大都是对一些基本概念的解释，辅以一些简单的例子，读完常常感觉自己已经明白了，但是在实际项目中遇到还是会很疑惑。

## BFC定义
Block Formatting Context(BFC)是一个独立的渲染区域，只有Block-Level box参与，它规定了内部的Box如何布局，并且与这个区域外部毫不相干。

## 如何生成BFC
* 设置浮动
* 绝对定位元素
* inline-block
* table-cell
* table-caption
* 设置overflow且值不为visible
* display:flex 或者 display:inline-flex

## BFC布局规则

* 内部的块级元素会在垂直方向一个接一个地放置。
* 块级元素垂直方向的距离由margin决定。属于同一个BFC的两个相邻块级元素的margin会发生重叠。
* 每个块级元素的margin box的左边，与包含块border box的左边相接触（对于从左往右的格式化，否则相反）。即使存在浮动也是如此（虽然盒子内的文字会环绕浮动元素），除非该块级元素形成了一个新的BFC。
* BFC的区域不会与float box重叠。
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
* 计算BFC的高度时，浮动元素也参与计算。

## 使用BFC防止外边距塌陷

上一节提到，在同一个BFC中的两个相邻块级元素的外边距会发生塌陷，如下边这个例子：

```html
<div class="container">
  <p>Sibling 1</p>
  <p>Sibling 2</p>
  <div class="newBFC">
    <p>Sibling 3</p>
  </div>
</div>
```

`div.container`是一个BFC，里面有三个块级元素，设置了相同的margin，其中第三个块级元素外面套了一层div，但是未设置任何样式。初始的CSS样式如下：

```css
.container {
  background-color: red;
  overflow: hidden;
}

p {
  margin: 10px 0;
  background-color: lightgreen;
}
```

初始结果如下：

![边距塌陷](http://upload-images.jianshu.io/upload_images/3623238-2490622e91c17367.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到相邻两个`p`元素间的间距为10px而不是20px，可见发生了外边距塌陷。如果要避免外边距塌陷，只需要使第三个`p`元素的外部容器形成一个新的BFC：

```css
.newBFC {
  overflow : hidden;  /* 形成新的 BFC */
}
```

修改后的效果为：

![防止边距塌陷](http://upload-images.jianshu.io/upload_images/3623238-6de3856a4bc8db3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 使用BFC包裹浮动元素

当容器中包含有浮动元素时，由于浮动元素会脱离页面的正常流，容器的高度会塌陷。解决容器高度塌陷主要有两种方法，一种是通过[clear fix](http://fengyu90.com/2016/09/15/css-float/#section-3)，另一种就是使容器形成块级格式上下文。

![BFC解决容器高度塌陷](http://upload-images.jianshu.io/upload_images/3623238-0396aa33c5ecfe10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 使用BFC避免文字环绕

我们知道，页面中的文字会环绕在浮动元素周围，但是有些时候我们想避免文字环绕的出现，例如以下这种情况：

![文字环绕](http://upload-images.jianshu.io/upload_images/3623238-d9c927c0708bb258.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

要实现图二的效果，同样可以利用BFC，不过首先我们需要理解文字环绕的形成机理。

```html
<div class="container">
  <div class="floated">
    Floated div
  </div>
  <p>
    Quae hic ut ab perferendis sit quod architecto, 
    dolor debitis quam rem provident aspernatur tempora
    expedita.
  </p>
</div>
```

这段HTML形成的效果如下：

![浮动元素](http://upload-images.jianshu.io/upload_images/3623238-9afd042277f06d6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，尽管浮动元素的存在导致了文字的偏移，`p`元素的左侧依然紧挨着容器左侧。当`p`元素中的文字增加到超出浮动元素高度时，超出部分的文字不再需要为浮动元素“让位”，便会重新从`p`元素的左侧开始显示，这样就形成了文字环绕的效果。如果想避免文字环绕，我们需要让整个`p`元素为浮动元素“让位”，而不仅仅是`p`元素中的文字。回忆BFC的布局规则中的一条：

> 每个块级元素的margin box的左边，与包含块border box的左边相接触（对于从左往右的格式化，否则相反）。即使存在浮动也是如此（虽然盒子内的文字会环绕浮动元素），除非该块级元素形成了一个新的BFC。

从这一条规则可以得出结论：**如果`p`元素形成了一个新的BFC，那么它就不再需要紧挨容器的左边缘**。

因此上面这个例子中，想避免`p`元素环绕浮动元素，只需要为`p`元素添加`overflow: hidden`，形成一个新的块级格式上下文。

## 使用BFC实现多列布局

在实现多列布局时，通常只需要将每一列浮动，并保证所有列（包括间距）的总宽度正好等于页面宽度。可以在实践中发现，多列布局的最后一列在某些浏览器中会换到下一行显示，这是由于多列布局中的列宽和间距通常是用百分比表示，而浏览器根据百分比计算盒模型宽度时四舍五入的方式有所不同，因此造成了总列宽大于了容器的宽度。

为了避免这种情况的出现，最后一列可以不适用浮动，而是通过形成一个新的BFC实现和浮动类似的效果。

HTML代码如下：

```html
<div class="container">
  <div class="column">column 1</div>
  <div class="column">column 2</div>
  <div class="column">column 3</div>
</div>
```

CSS代码如下：

```css
.column {
  width: 31.33%;
  background-color: green;
  float: left;
  margin: 0 1%;
}

/* 最后一列形成新的BFC */
.column:last-child {
  float: none;
  overflow: hidden; 
}
```

最终效果如下：

![多列布局](http://upload-images.jianshu.io/upload_images/3623238-509d681203679723.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当然这种方法并不一定是最优的解决方案，只是以此说明BFC的一种可能的应用。

## 参考资料

[1] [Understanding Block Formatting Contexts in CSS](https://www.sitepoint.com/understanding-block-formatting-contexts-in-css/)

[2] [Block formatting context](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)
 






	
	



	












