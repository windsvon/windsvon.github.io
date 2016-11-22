---
layout:     post
title:      "前端布局之外边距塌陷问题"
date:       2016-10-11
author:     "Yu"
header-mask: 0.6
tags:
    - 前端布局
---

块级元素的上下外边距有时候会合并，合并后的外边距等于**合并前两个外边距中的较大值**，这种现象称为**外边距塌陷**。

外边距塌陷有以下三种情况：

## 同级相邻元素

同级相邻元素之间的外边距会塌陷，塌陷后的间距等于两个元素外边距的较大值（如果后者被清除浮动，不遵循此规则），例如：

```css
h1 {
  margin: 0 0 25px 0;
  background: #cfc;
}
p {
  margin: 20px 0 0 0;
  background: #cf9;
}
```


![同级元素外边距塌陷](http://upload-images.jianshu.io/upload_images/3623238-03bb73a5fea4fce2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 父子元素间的边距塌陷

如果块级父元素中，不存在**border, padding, 行内元素以及清除浮动**这四条属性（对于border和padding，也可以说，当上border及上padding宽度为0时），那么这个块级元素和其第一个子元素的**上外边距（margin-top）**就可以说”挨到了一起“。此时这个块级父元素和其第一个子元素就会发生 **上外边距塌陷** 现象，换句话说，此时这个父元素对外展现出来的外边距将直接变成这个父元素和其第一个子元素的margin-top的较大者。

类似地，若块级父元素的**下外边距（margin-bottom）**与它的最后一个子元素的**下外边距**之间没有父元素的**border, padding, 行内元素，height, min-height, max-height**分隔时，就会发生 **下外边距塌陷** 现象。

下面是一个上外边距合并的例子：

```html
<h1>Heading Content</h1>
<div>
  <p>Paragraph content</p>
</div>
```

```css
h1 {
  margin: 0;
  background: #cff;
}
div {
  margin: 40px 0 25px 0;
  background: #cfc;
}
p {
  margin: 20px 0 0 0;
  background: #cf9;
}
```

这段代码的输出结果为：

![父子元素外边距塌陷](http://upload-images.jianshu.io/upload_images/3623238-562e3a2c62b4600a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到在正文元素中，父元素的上外边距为40px，子元素的上外边距为20px，然后最终正文和标题之间的距离确是40px而不是60px。因为父子元素间发生了外边距塌陷。为了避免边距塌陷，只需要将父元素和子元素的外边距“分隔开”，例如为父元素添加一个边框：

```css
h1 {
  margin: 0;
  background: #cff;
}
div {
  margin: 40px 0 25px 0;
  background: #cfc;
  border-top: 1px solid #000;
}
p {
  margin: 20px 0 0 0;
  background: #cf9;
}
```

输出结果为：

![防止父子元素边距塌陷](http://upload-images.jianshu.io/upload_images/3623238-52e3dc6d772374d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 空的块级元素

当一个空的块级元素的**上边距（margin-top）**和**下边距（margin-bottom）**之间没有**border，padding，行内元素，height，min-height**分隔时，上下边距会塌陷。

## 小结

* 以上几种情况同时出现会产生更复杂的边距塌陷问题。
* 这些规则在边距为0时同样适用，因此父元素中的第一个和最后一个子元素的外边距总是会超出父元素（满足上述几种情况时），无论父元素的边距是否为0。
* 当使用负边距时，塌陷后的边距等于最大的正边距和最大负边距的代数和。
* 浮动元素和绝对定位元素永远不会塌陷。












