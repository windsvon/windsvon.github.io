---
layout:     post
title:      "响应式开发中的图片处理"
date:       2016-11-02
author:     "Yu"
header-mask: 0.6
catalog: true
tags:
    - 前端优化
    - 翻译
---

翻译自[Peter LePage](https://developers.google.com/web/fundamentals/design-and-ui/media/images)

一个网页中平均约60%的数据量是被图片占据的，所以在响应式开发中注重图片的优化可以很大程度上提高网页的性能，例如为低分辨率的屏幕推送低分辨率的图片可以避免流量的浪费。

## 图片标签

HTML中的`img`元素负责下载，解码并加载图片内容。做到以下几点可以帮助网页实现更好的体验：

### 使用相对尺寸

使用相对尺寸定义图片可以防止其超出可视范围。例如设置`width: 50%`可以保证图片的宽度是其父元素的50%（不是可视范围的50%也不是其实际尺寸的50%）。

因为CSS允许内容超出容器，可以使用`max-with: 100%`来避免图片以及其他内容超出容器范围。

### 利用`srcset`为高分辨率屏幕加载高质量图片

利用`img`元素的`srcset`属性可以很方便地在不同的屏幕加载不同的图片文件。浏览器会根据设备的特定选择最佳的图片来显示，例如在retina屏幕上加载双倍分辨率的图片，或者在网速不好的情况下加载较小的图片。

``` html
<img src="photo.png" srcset="photo@2x.png 2x" ...> 
```

对于不支持`srcset`的浏览器, `img`元素会加载`src`指向的图片。对于支持`srcset `的浏览器，在请求页面之前会先解析`srcset`的内容（图片和相应的加载条件），只有最合适的图片会被下载并显示。

理论上图片加载条件可以是像素密度（pixel density）或屏幕高宽，但是现在只有像素密度的支持性比较好。

### 使用`picture`元素

`picture` 元素可以根据不同的设备特性加载不同的图片版本，例如设备尺寸，分辨率，方向等等。

一个`picture`元素中可以包含多个`source`元素，分别对应在不同情况下需要加载的图片。

``` html
<picture>
  <source media="(min-width: 800px)" srcset="head.jpg, head-2x.jpg 2x">
  <source media="(min-width: 450px)" srcset="head-small.jpg, head-small-2x.jpg 2x">
  <img src="head-fb.jpg" srcset="head-fb-2x.jpg 2x" alt="a head carved out of wood">
</picture>
```

在这个例子中，当浏览器宽度大于800像素时会根据屏幕分辨率加载`head.jpg`或者`head-2x.jpg`。当浏览器宽度介于450像素与800像素之间时，会根据屏幕分辨率加载`head-small.jpg`或`head-small-2x.jpg`。当浏览器宽度小于450像素或者浏览器不支持`picture`元素时，浏览器会渲染`img`元素，**因此保证`picture`元素中包含有一个`img`元素**。

### 相对尺寸图片

在一个响应式页面中，图片尺寸可能随着浏览器的缩放而改变，因此无法预先知道图片的有效像素密度。可以通过为图片文件添加宽度描述来使浏览器自动计算像素密度从而加载最合适的图片。

```html
<img src="lighthouse-200.jpg" sizes="50vw"
     srcset="lighthouse-100.jpg 100w, lighthouse-200.jpg 200w,
             lighthouse-400.jpg 400w, lighthouse-800.jpg 800w,
             lighthouse-1000.jpg 1000w, lighthouse-1400.jpg 1400w,
             lighthouse-1800.jpg 1800w" alt="a lighthouse">
```

这个例子会加载一张宽度为浏览器可视区域宽度一般的图片，并且根据浏览器宽度和设备像素比选择合适的图片加载


![相对尺寸的图片](http://upload-images.jianshu.io/upload_images/3623238-bf078bd8c1814083.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在很多情况下，图片布局也会随着浏览器尺寸的变化而改变，例如在小屏上要求图片撑满可视区域宽度，然而在大屏幕上要求图片只占可视范围的一小部分。

```html
<img src="400.png" 
     sizes="(min-width: 600px) 25vw, (min-width: 500px) 50vw, 100vw"
     srcset="100.png 100w, 200.png 200w, 400.png 400w,
             800.png 800w, 1600.png 1600w, 2000.png 2000w" alt="an example image">
```

在这个例子中，当浏览器宽度大于600像素时，图片宽度是可视区域宽度的25%；当浏览器宽度在500像素到600像素之间时，图片宽度是可是宽度的50%；当浏览器宽度小于500像素时，图片撑满可视区域宽度。

## CSS中的图片

CSS中的`background`属性可以使图片通过多种方法呈现，另外通过CSS3中的媒体查询也可以响应式地加载图片。

### 使用媒体查询动态加载背景图片

使用媒体查询可以虽然浏览器窗口缩放加载不同版本的图片

```css
.example {
  height: 400px;
  background-image: url(small.png);
  background-repeat: no-repeat;
  background-size: contain;
  background-position-x: center;
}

@media (min-width: 500px) {
  body {
    background-image: url(body.png);
  }
  .example {
    background-image: url(large.png);
  }
}
```

### 使用`image-set`提供高分辨率图片

使用CSS的image-set()函数可以根据设备特性加载不同版本的图片，作用和`<img>`标签的`srcset`属性相似

```css
background-image: image-set(
  url(icon1x.jpg) 1x,
  url(icon2x.jpg) 2x
);
```

`image-set()`不仅控制图片的加载，而且会根据加载的图片进行缩放缩放。浏览器默认`2x`的图片尺寸是`1x`图片的两倍，因此会缩小`2x`的图片使得显示出的图片大小相同。

由于`image-set()`只支持Chrome和Safari浏览器，所以在使用时要考虑到浏览器的兼容性，例如：

```css
.sample {
  width: 128px;
  height: 128px;
  background-image: url(icon1x.png);
  background-image: -webkit-image-set(  
    url(icon1x.png) 1x,  
    url(icon2x.png) 2x  
  );  
  background-image: image-set(  
    url(icon1x.png) 1x,  
    url(icon2x.png) 2x  
  );
}
```

### 使用媒体查询加载高分辨率图片

媒体查询可以使用[设备像素比](http://www.xingzai.org/css-note/devicepixelratio.html)作为规则，在高分屏幕上加载高分辨率图片，例如：

```css
@media (min-resolution: 2dppx),
(-webkit-min-device-pixel-ratio: 2)
{
  /* 高分辨率的图片资源 */
}
```
这个例子中的两个判断条件其实是等价的，其中`min-resolution`是标准写法，但是只有Chrome, Firefox和Opera浏览器支持。

 






	
	



	












