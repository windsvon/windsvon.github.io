---
layout:     post
title:      "理解CSS中的浮动"
date:       2016-09-15
author:     "Yu"
header-mask: 0.6
catalog: true
tags:
    - 前端布局
---

**浮动（float）**是CSS中一个重要的定位及布局属性，`float` 属性可以接受以下几个值：

* `none` ：元素不浮动，默认值。
* `left` ：将元素浮动到容器左侧。
* `right`：将元素浮动到容器右侧。
* `inherit`：元素继承其父元素的浮动方向。

**注意：浮动的元素会自动添加`display: block`。**

## 什么是浮动

要理解什么是浮动，可以参考传统的印刷设计。在传统印刷布局中，可以设置图片被文字环绕。如下所示：

![传统印刷布局](http://upload-images.jianshu.io/upload_images/3623238-4ce5740d38f92023.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在传统页面布局软件（例如MS Word）中，文本框可以设置是否要文字环绕。如果忽略文字环绕，图片会被认为脱离了文本流，这时文字会穿过图片，好像图片并不存在。

![网页布局](http://upload-images.jianshu.io/upload_images/3623238-eb45d7f1cd625feb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在网页设计中，设置了浮动的元素就好像传统印刷布局中设置了文字环绕的图片。和绝对定位元素不同，浮动元素**虽然脱离了正常的文档流，但并没有脱离文本流**，意思是正常文档流中的块级元素会按照浮动元素不存在一样排列，但是文本并不会和浮动元素重叠。

与之相对，绝对定位元素完全脱离了页面的文档流和文本流，类似传统印刷中忽略文字环绕的情况。绝对定位的元素不会影响其他元素的位置也不会受其他元素的影响。下面是一个浮动前后对比的例子：

**图 | 浮动前：**
![浮动前](http://upload-images.jianshu.io/upload_images/3623238-d1d8c6abbbe79bac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**图 | 浮动后：**
![浮动后](http://upload-images.jianshu.io/upload_images/3623238-fcd01d10bee81eab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。**


## 浮动布局

除了实现简单的文字环绕效果，浮动还能够用来做整个页面的布局，如下所示：


![页面布局](http://upload-images.jianshu.io/upload_images/3623238-ae7c901f83c3f044.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

除此之外，相对于绝对定位元素，使用浮动元素可以使布局更加灵活，例如下面这个使用浮动布局的名片，当照片尺寸发生变化时，右侧的文字会自适应重新布局，不影响整体显示效果：


![浮动布局](http://upload-images.jianshu.io/upload_images/3623238-c56f275e03cf41c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然而如果使用绝对定位的照片，虽然初始的显示效果是相同的，但是当照片尺寸变化时，文字就可能被遮挡，显然这种布局的灵活性并不好，如下所示：


![绝对定位布局](http://upload-images.jianshu.io/upload_images/3623238-f251fc74be021143.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 清除浮动

说到浮动就不能不提它的兄弟属性`clear`。`clear` 属性指定：一个元素是紧挨着前面的浮动元素，还是必须移动到它们的下面。CSS属性既可以用于浮动元素，也可以用于非浮动元素。

当应用于非浮动元素时，它将元素的**边框边界**移动到所有相关浮动元素**外边界**的下方，这时候外边距塌陷不起作用。

当应用于浮动元素时，它将元素的**外边界**移动到所有相关浮动元素**外边界**的下方。这会影响后面浮动元素的布局，后面的浮动元素的位置无法高于它之前的元素。

**图 | 清除浮动前**
![清除浮动前](http://upload-images.jianshu.io/upload_images/3623238-c47ba19c139cf9c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


上面这个例子中， sidebar浮动到了右侧，并且其长度小于正文容器。如果footer也设置了浮动，就可能出现如上图所示的情况。为了避免这种情况出现，需要用到清除浮动，使footer移动到所有浮动元素的下方。


**图 | 清除浮动后**
![清除浮动后](http://upload-images.jianshu.io/upload_images/3623238-cadfe33dbb01011e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里为footer设置的是`clear: both`，`clear`属性有四个可能的参数，最常用的就是`both`，会清除两个方向的浮动。使用`left` 和 `right`可以清除相应方向上的浮动。`none` 是默认值，`inherit`表示该元素继承其父元素的`clear`属性的值。


## 容器的塌陷问题

使用浮动时遇到的一个很棘手的问题是浮动对容器的影响。如果容器内除了浮动元素外没有其他元素，那么它的高度就会塌陷为0。如果父元素背景透明的话，这种塌陷并不容易被觉察到，如下所示：

![容器的塌陷](http://upload-images.jianshu.io/upload_images/3623238-cbdafcac2d95aa8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果想避免容器的塌陷，可以将容器浮动（形成块级格式上下文），或者在浮动元素和容器尾部之间设置清除浮动，后者可以通过伪元素 `:after`实现：

```css
#container:after {
 content: "";
 display: block;
 clear: both;
}
```

## 清除浮动的技巧

在一些复杂的情况下，需要一些特别的方法来清除浮动。

* **使用空的DIV元素**

	顾名思义，就是在需要清除浮动的位置放置一个空的div。
	
```html
<div style="clear:both;"></div>
```

* **设置overflow**

	通过为父元素设置`overflow`为`auto` 或 `hidden`可以使父元素将浮动元素包含在内，避免了浮动元素对之后元素的影响。这个方法的原理也是形成了**块级格式上下文**。

* **利用伪类`:after`**

	利用伪类`:after`可以实现清除浮动的目的，只需要为容器添加一个类，如"clearfix"，然后定义如下css样式：
	
```css
.clearfix:after { 
    content: "";
    visibility: hidden;
    display: block;
    height: 0;
    clear: both
}
```

举个例子说明，例如以下一组色块：

**图 | 清除浮动前**
![清除浮动前](http://upload-images.jianshu.io/upload_images/3623238-f54618f13e5ede65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果想实现每一行一种颜色的效果，有两种策略：

* 为每种颜色的四个色块外套一层div，然后为div设置 `overflow` 为 `auto` 或 `hidden`，或像上面提到的利用伪类 `:after`。

* 在每种颜色的最后一个色块后面添加一个空的div，并为其设置 `clear: both`。

最后的效果如下：

**图 | 清除浮动后**
![清除浮动后](http://upload-images.jianshu.io/upload_images/3623238-83159e4553414718.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 参考文献

[1] [float](https://css-tricks.com/almanac/properties/f/float/)

[2] [clear](https://developer.mozilla.org/en-US/docs/Web/CSS/clear)

[3] [CSS 浮动](http://www.w3school.com.cn/css/css_positioning_floating.asp)











