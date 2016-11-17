---
layout:     post
title:      "使用gulp进行图片优化"
date:       2016-11-12
author:     "Yu"
header-mask: 0.6
tags:
    - 前端优化
    - 翻译
---

之前[文章](http://www.jianshu.com/p/beab26ded951)已经提到，图片的压缩处理可以很大程度提高网页性能。在实际工程中，借助自动化工具可以很方便地实现图片的压缩，本文主要介绍gulp工作流下图片的压缩方法。

## 准备工作

使用gulp进行图片压缩首先要寻找合适的插件，本文主要介绍两种：

* [gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)
* [gulp-smushit](https://www.npmjs.com/package/gulp-smushit)

然后再工作目录下创建两个文件夹，分别保存原始图片和压缩后的图片，原始图片如下：

**图 | Taipei.jpg, 3456 x 2304 ~ 1.2MB**
![Taipei.jpg, 3456 x 2304 ~ 1.2MB](http://upload-images.jianshu.io/upload_images/3623238-a6eece3ab8f6f39f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**图 | winkpax.png, 1372 x 1178 ~ 1.5MB**
![winkpax.png, 1372 x 1178 ~ 1.5MB](http://upload-images.jianshu.io/upload_images/3623238-af4f7cbc1d7a7ac1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## gulp-imagemin

这个插件可以用来压缩PNG, JPEG, GIF和SVG图片

优势：

* 有很多定制选项
* 可以引入更多第三方优化插件，例如[pngquant](https://www.npmjs.com/package/imagemin-pngquant) (见下面例子)
* 可以处理多种图片格式

劣势：

* 没有`verbose`输出选项

### Demo

`gulpfile.js` （包含pngquant）

```js
var gulp = require('gulp');
var imagemin = require('gulp-imagemin'),
    pngquant = require('imagemin-pngquant');

gulp.task('imagemin', function () {
    return gulp.src('src/*')
        .pipe(imagemin({
            progressive: true,
            use: [pngquant()]
        }))
        .pipe(gulp.dest('imagemin-dist'));
});
```
输出结果如下，可以看到图片体积减小了57.6KB (2.2%)

```cmd
[10:37:25] Using gulpfile ~/Desktop/Work/Practice/gulp-compressor/gulpfile.js
[10:37:25] Starting 'imagemin'...
[10:37:39] gulp-imagemin: Minified 2 images (saved 57.6 kB - 2.2%)
[10:37:39] Finished 'imagemin' after 14 s
```

## gulp-imagemin

Smushit是Yahoo开发的一款用来优化PNG和JPG的插件，它的原理是移除图片文件中不必要的数据。这是一个无损压缩工具，这意味着优化不会改变图片的显示效果和质量。

优势：易配置

劣势：只能处理JPG和PNG

### Demo

`gulpfile.js`

```js
var gulp = require('gulp');
var smushit = require('gulp-smushit');

gulp.task('smushit', function () {
    return gulp.src('src/*')
        .pipe(smushit({
            verbose: true
        }))
        .pipe(gulp.dest('smushit-dist'));
});
```

输出结果：

```cmd
[10:48:26] Using gulpfile ~/Desktop/Work/Practice/gulp-compressor/gulpfile.js
[10:48:26] Starting 'smushit'...
[10:50:31] /Users/fengyu/Desktop/Work/Practice/gulp-compressor/src/IMG_3881.jpg
[10:50:31] gulp-smushit: Compress rate % 0
[10:50:31] gulp-smushit: 1188874 bytes  to   1188223 bytes
[10:51:09] /Users/fengyu/Desktop/Work/Practice/gulp-compressor/src/winkpax@2x.png
[10:51:09] gulp-smushit: Compress rate % 69
[10:51:09] gulp-smushit: 1458533 bytes  to   446429 bytes
[10:51:09] Finished 'smushit' after 2.7 min
```
这个例子可以看出smushit对PNG图片具有更好的压缩效果，压缩率高达69%，而且压缩前后图片质量没有明显的改变（如下图），这足以说明图片压缩的必要性。

**图 | 压缩前 ~1.5MB**
![压缩前 ~1.5MB](http://upload-images.jianshu.io/upload_images/3623238-8d59861b6bd1d68a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**图 | 压缩后 ~446KB**
![压缩后 ~ 446KB](http://upload-images.jianshu.io/upload_images/3623238-88b2df8c40cc0be1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 总结

可以看到不同插件对于不同的图片格式的压缩效果不尽相同，实际工程中应当尝试多种插件，找到最优的图片压缩方案。

参考资料：

[diezjietal's blog](http://diezjietal.be/blog/2015/02/18/image-optimizers.html)


	
	



	












