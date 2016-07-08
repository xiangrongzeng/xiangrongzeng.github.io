---
layout: post  
title: 搭建博客  
categories: [blog ]  
tags: [blog, ]  
description: 「这个博客是怎么搭建起来的」   
---

简单记录这个博客是怎么搭建起来的

[ TOC ]

## 前言
折腾了几天，尝试用github来搭建自己的个人博客，一直没有找到好的方法。今天看到了这位博主的[搭建教程](http://cnfeat.com/blog/2014/05/10/how-to-build-a-blog/)，总算是有所进展，现在试试将自己的经历写下来。
 ## 简单直接的方法

网上有很多帖子讲了要怎么使用github的page服务来搭建个人网站。但是涉及到很多东西。这里介绍最简单的办法——直接fork一个现成的blog。

### Git和GitHub的相关内容（略）
这里略去了如何创建github账号，以及git、github的基本用法。

### fork现成的blog
我们首先在GitHub中找到自己想要fork的博客仓库。比如[这个](https://github.com/cnfeat/blog.io/tree/master)，然后我们将它fork到我们自己的GitHub上。这时你的GitHub仓库中就多了一个叫blog.io的仓库。假设你的GitHub用户名是jack，因为GitHub Pages服务的原因，我们需要将刚刚fork的blog.io的名字修改为：jack.github.io。这样修改好之后，就可以通过
> https://jack.github.io/

来访问你的博客啦。这里要注意的是，一定要用你的用户名来给仓库命名。 

## 发布新博文
在_posts文件夹中新建文件，文件命名是有要求的，必须是日期+英文文件名+md。比如：2017-07-07-how-to-build-blog.md。

建立新的文件后就可以开始写作了。但要注意的是每篇博文的最开头一定要插入一段代码，指明博文的相关属性，这样github才能够正确解析它，用户也才能看到正确显示的格式。通常需要设置的信息有如下几个：

	---
	layout: post  
	title: 搭建博客  
	categories: [blog ]  
	tags: [blog, ]  
	description: 「这个博客是怎么搭建起来的」   
	---

 - layout: 表示你要用的模版，一般就用默认的就行
 - title：就是你的这篇博文的名字
 - categories：表示这篇博文所属的类别
 - tags：表示这篇博文要打的标签，如果有多个标签，需要用英文半角逗号分隔开
 - description：就是概括下整篇博文的主要内容

## 如何插入公式
如何在markdown中插入公式，并且能够被服务器解析后正常显示呢？我们需要用到Mathjax(https://www.mathjax.org/)来帮助我们渲染公式。使用方式很简单，只需要把下面这段代码放到我们博文生成的html文件的\<head\>标签块中。

	<script type="text/javascript" async
        src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
    </script>

如下图所示：
![](https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/inject_mathjax.jpg)

现在的问题是，我们编写的是.md文件，我们并不想手工将.md文件转为.html文件然后再手工向其中添加这段代码。我们希望能够自动将这段代码插入进去。怎么做呢？

最好的办法是找到模版文件，然后在模版中添加这段代码，这样程序就能够自动将这段代码添加进去。一般来说，模版文件在_layouts文件夹中，名称一般是固定的default.html。我们在default.html中添加上述代码就可以了。有时候default.html中并没有直接的设置语句，而是引用了其它地方的文件，那么我们找到引用的文件，再在其中添加上述代码即可。

其实上述代码不一定非得放在\<head\>标签块中，因此我们可以直接将这段代码放在我们的.md文件中。这样做并不推荐，但是如果你实在找不到怎么在模板中添加上述代码，这也不失为一种方法。

接下来就可以直接用LaTex的语法来写公式了，唯一一点要注意的是，行内公式也需要使用两个连着的**\$\$**符号来表示公式环境，只用一个\$符是不能表示公式的。下面是几个例子：

	$$\alpha$$
	$$\in$$

分别表示$$\alpha$$$$\in$$。
