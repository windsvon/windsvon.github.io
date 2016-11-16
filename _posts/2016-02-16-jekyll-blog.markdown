---
layout:     post
title:      "Jekyll搭建静态博客"
date:       2015-09-22
author:     "Yu"
header-mask: 0.6
tags:
    - 前端开发
---

开始学习前端之后渐渐感觉前端的知识多且杂，因此一直想以博客的形式对所学知识及时总结，相信会对学习过程有所帮助。作为一名面向逼格编程的程序猿，用博客园那种东西显然无法满足我。由于之前在知乎了解到可以通过jekyll和Github Pages搭建博客，因此决定试一试，好在英文不错，根据各种官方非官方教程很快做出了这个简单的博客。

## 1、Github Pages

Github Pages是Github团队为了让开发者能够更加方便的展示项目而推出的功能，之前看过评论说用Github Pages搭建博客是投机取巧的行为。我专门查阅了[Github Pages官网](https://pages.github.com/),在页面最下方专门强调了博客功能，并推荐使用jekyll来搭建博客。所以就放心的使用啦。

使用Github Pages的方法非常简单，只需要在github上建立名为`username.github.io`的仓库，并向其中导入带有`index.html`的网页源文件，就可以通过网址`https://username.github.io`来访问了。`index.html`中的内容将显示在主页上。

对于普通仓库，也可以建立名为`gh-pages`的branch，然后通过`https://username.github.io/仓库名`来访问仓库内容，十分方便。



## 2、jekyll简介

>Jekyll is a parsing engine bundled as a ruby gem used to build static websites from dynamic components such as templates, partials, liquid code, markdown, etc. Jekyll is known as "a simple, blog aware, static site generator".

以上是官网上对jekyll的介绍，可知jekyll的作用主要是搭建静态博客页面。可以使用方便的Markdown语言，且不用调用任何数据库。常用后端功能如评论功能可以通过第三方插件来实现。

## 3、安装方法

jekyll的安装方法非常简单，但是需要事先安装一些语言环境：

 * [Ruby](http://www.ruby-lang.org/en/downloads/)
 * [RubyGems](https://rubygems.org/pages/download)
 * Linus, Unix, 或者Mac OSX
 * [NodeJS](https://nodejs.org/en/)
 * [Python](https://www.python.org/downloads/)

 之后可通过如下命令使用RubyGems来安装Jekyll
 
```
$ gem install jekyll
```

然后通过如下命令即可在当前文件夹创建一个新的博客

```
$ jekyll new myblog
```

如此一个简单的博客就创建完成了，在myblog文件夹下，输入如下命令：

```
$ jekyll serve
```

然后即可在浏览其中输入localhost:4000来查看新建的博客。

## 4、使用方法

在myblog文件夹下可看到如下的文件结构：

 * _config.yml
 * _includes
 * _layouts
 * _posts
 * _sass
 * about.md
 * css
 * feed.xml
 * index.html

其中index.html中的内容会显示在博客主页上，在`_config.yml`中可以进行博客的基本设置。jekyll采用的是模块化的开发方式，页面的各个部分可以在独立的html文件中编写，并保存在`_layouts`和`_includes`文件夹中，然后通过jekyll的语法拼装出页面。具体的语法请参考[jekyll官网](http://jekyllrb.com/)。

博文可以通过markdown语言来编写并保存在`_posts`文件夹中，注意博文的命名必须按照`YYYY-MM-DD-my-blog-name.markdown`来书写。

在本地制作好博客之后即可通过git命令push到`username.github.io`仓库中。在浏览器中输入`https://username.github.io`就可以看到自己的作品了~





