---
layout: post
title: 搭建博客
categories: [blog, ]
tags: [blog]
---





##前言

折腾了几天，尝试用github来搭建自己的个人博客，一直没有找到好的方法。今天看到了这位博主的[搭建教程](http://cnfeat.com/blog/2014/05/10/how-to-build-a-blog/)，总算是有所进展，现在试试将自己的经历写下来。

##简单直接的方法

网上有很多帖子讲了要怎么使用github的page服务来搭建个人网站。但是涉及到很多东西。这里介绍最简单的办法——直接fork一个现成的blog。

###Git和GitHub的相关内容（略）

###fork现成的blog
我们首先在GitHub中找到自己想要fork的博客仓库。比如[这个](https://github.com/cnfeat/blog.io/tree/master)，然后我们将它fork到我们自己的GitHub上。这时你的GitHub仓库中就多了一个叫blog.io的仓库。假设你的GitHub用户名是jack，因为GitHub Pages服务的原因，我们需要将刚刚fork的blog.io的名字修改为：jack.github.io。这样修改好之后，就可以通过
>https://jack.github.io/

来访问你的博客啦。这里要注意的是，一定要用你的用户名来给仓库命名。
