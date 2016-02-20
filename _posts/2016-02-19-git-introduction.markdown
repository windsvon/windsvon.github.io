---
layout: post
title:  "常用的Git操作"
date:   2016-02-18 05:45:08
categories: 基础
comments: true
---

<br>

#### 之前版本控制只会用github的桌面客户端**Github Desktop**，虽然也能满足需求，但是完全满足不了我的逼格，于是系统地学习了一下git的基本操作，在这里总结一下。 

1、安装Git
---------------
由于之前安装的github的客户端自带了git，因此直接就可以用了，开始还以为是mac系统自带的。后来看了些资料才知道可以在[git官网](http://git-scm.com/download/mac)下载并安装，也是很方便的。

2、基本命令
---------------
下列基本命令主要是针对本地仓库的控制：

* `git init`： 创建新的Git仓库（将当前文件夹变为Git仓库）
* `git status`： 查看当前工作目录以及staging area（暂存区域）的内容
* `git add`： 将文件添加到暂存区域
* `git diff`： 查看当前工作目录和暂存区域之间的区别
* `git commit`： 将暂存区域中的内容永久的替换到仓库中
* `git log`： 列出之前所有的commit

3、撤销操作
---------------
在使用git的时候常常遇到需要撤销操作的情况，此时可以通过下列命令来实现

* `git checkout HEAD filename`： 撤销`filename`在上一次commit之后的所有改动
* `git reset HEAD filename`： 将`filename`的改动移出暂存区域，但是**不撤销改动**
* `git reset SHA`： 将工作目录恢复到某次commit之后的状态，SHA可通过`git log`查看，改命令只需要输入SHA的**前7个**字符

4、分支
----------------
Git的分支功能是其实现版本控制的主要手段，也是平时最常用的功能之一。通过建立分支，可以在同一个文件夹下管理多个版本的代码，而不需要重新建立文件夹。

* `git branch`： 列出当前文件夹下的所有分支名称
* `git branch branch_name`： 建立名为`branch_name`的新分支
* `git checkout branch_name`： 将工作目录转到`branch_name`分支下
* `git merge branch_name`： 将`branch_name`分支下的改动覆盖到当前工作目录下
* `git branch -d branch_name`： 删除分支

5、远程协作
------------------
通过使用git，可以实现多人协作开发项目，大大提高了团队协作的效率。此外在github上托管项目时也会用到本节介绍的命令。

使用远程协作有两种方式，一种是直接使用`git clone`来克隆一个远程文件夹，然后在该文件夹下工作；另一种方式是使用`git remote add orgin remote_address`，来为本地文件夹定义一个远程地址。以使用github为例，我想建立一个名为CV的仓库，并在本地实现远程控制。如果采用第一种方式，应该先在github上建立名为CV的仓库，然后在本地的terminal中输入

{% highlight python %}
$ git clone https://github.com/username/CV.git
{% endhighlight %}

即可在本地得到一个名为CV的文件夹。如果采用第二种方式，同样要在github上建立名为CV的仓库，然后在本地建立一个名为CV的文件夹，在本地CV文件夹下，输入

{% highlight python %}
$ git remote add origin https://github.com/username/CV.git
{% endhighlight %}

两种方法完成后，在本地CV文件夹中输入`git remote -v`均可以得到如下内容

{% highlight terminal %}
origin  https://github.com/username/CV.git (fetch)
origin  https://github.com/username/CV.git (push)
{% endhighlight %}

证明本地文件已经和远程文件相连了，接下来是一些远程控制的常用操作：

* `git fetch`：将远程文件夹中的内容获取到本地
* `git merge origin/master：将远程文件的内容覆盖当前本地分支
* `git push origin branch_name：将当前工作分支推送到远程分支`branch_name`
，并覆盖远程分支








