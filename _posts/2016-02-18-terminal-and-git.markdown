---
layout: post
title:  "Mac OS下常用的Terminal命令"
date:   2016-02-18 05:45:08
categories: 基础
comments: true
---

<br>

##### 作为半路出家的程序员，花了不少时间才熟悉了Mac OS下常用的Terminal命令。写在这里希望能帮助其他初学者。 

之前作为工科狗虽然也经常接触编程，但是对于Command-Line这种看起来十分Geek的手段还是心有畏惧，并没有怎么接触过，最多知道一点`cd`，`cp`什么的。最近在[codecademy](https://www.codecademy.com/)的帮助下终于把一些常用功能搞明白了。

Command-Line其实就是用文本命令控制电脑的方式（text-interface)，和平时用鼠标控制并没有本质区别，同样可以实现对文件的增删改查、安装卸载等功能。只是对于程序员来说直接用命令控制操作系统更加高效一些。Terminal是Mac OS下的Command-Line界面。下文将分类列举常见的命令。

#### 定位文件
* `pwd`： **present working directory** 输出当前文件夹的名称
* `ls`： **list** 输出当前文件夹中所有文件的名称 
* `cd`： **change directory** 转换工作目录，例如Terminal中输入`$cd user/Desktop/CV`，当前工作文件夹会转到`CV`


#### 附加命令
* `ls -a`： 列出当前工作目录下包括隐藏文件的所有文件名称
* `ls -l`： 以**长名称**形式列出所有文件名称（包含创建日期等）
* `ls -t`： 根据最后一次修改时间排列文件

#### 添加，复制，移动，删除文件
* `mkdir`： **make directory** 创建新文件夹
* `touch`： 创建新文件
* `cp`： **copy** 复制文件，例如输入`$cp me.txt you.txt`将前者内容复制进后者，输入`$cp me.txt her/`将文件`me.txt`复制进文件夹`her`
* `mv`： **move** 移动或重命名文件，例如输入`$mv me.txt you.txt`会将前者文件名替换为后者
* `rm`： **remove** 删除文件
* `rm -r`： 删除文件夹

#### 其他的强大命令
* `cat`： 输出文件中的内容
* `echo`： 显示信息
* `sort`： 使文本文件中的所有行按照字母顺序排列
* `uniq`： 删除文本文件中重复的行
* `grep`： **global regular expression print** 利用正则表达式查找文件中的内容，并输出结果。例如输入`$grep city world_city.txt`将输出文件`world_city.txt`中所有包含字符`city`的行
* `sed`： **stream editor** 查找并替换文件中的内容。例如输入`$sed 's/city/town/' world_city.txt`会将`world_city.txt`中第一个`city`替换为`town`


#### 命令的redirection

通过redirection可以将多个命令连接起来，进一步提高工作效率。要理解redirection，首先要了解几个概念：

* 标准输入 **standard input**，简写为`stdin`
* 标准输出 **standard output**，简写为`stdout`
* 标准报错 **standard error**，简写为`stderr`

常用的redirection命令：

* `>`： 将左侧命令输出的内容右侧文件中，并覆盖文件中的原有内容
* `>>`： 将左侧命令输出的内容导入右侧文件中，添加到原有内容之后
* `<`： 将右侧文件的标准输入作为左侧命令的标准输入，例如`$cat < me.txt`会将`me.txt`作为`cat`命令的标准输入
* `|`： 将左侧文件的标准输出作为右侧文件的标准输入

限于篇幅，就写到这里，本文只是一些基本功能，但其实大部分功能博主平时也并没有用到过，以后会强迫自己多加使用，如果有其他常用命令而文中没有涉及的，欢迎留言~








