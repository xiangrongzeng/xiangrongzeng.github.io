---
layout: post  
title: vim的pymode插件
categories: [coding]  
tags: [vim, python ]  
description: 「介绍如安装vim的pymode插件」   
---

本文介绍如何将vim通过python-mode插件打造成python开发环境

## 安装pathogen.vim

安装方法很简单。到[这里](https://github.com/tpope/vim-pathogen)下载pythogen.vim，然后将其放到“~/.vim/autoload/”目录下（如果是windows，则是“C:\Users\你的用户名\vimfiles\autoload\”目录下）就行。

## 安装python-mode插件
安装方法同样很简单。官网在[这里](https://github.com/klen/python-mode#using-pathogen-recommended)。

### linux环境
执行如下命令（其实就是将python-mode仓库放到“~/.vim/bundle/”目录下）：

> % cd ~/.vim

> % mkdir -p bundle && cd bundle

> % git clone https://github.com/klen/python-mode.git

然后在自己的vim配置文件（~/.vimrc）中加入以下设置：

> " Pathogen load

> filetype off

> call pathogen#infect()

> call pathogen#helptags()

> filetype plugin indent on

> syntax on

这样就配置好了。

### windows环境
基本跟linux环境下的配置一样，唯一要修改的是将所有的“~/.vim/”替换为“C:\Users\你的用户名\vimfiles\”。同时注意.vimrc文件位于“C:\Users\你的用户名\”目录下。

如此就将插件安装好了。

## 其它
python-mode中的一些命令要用到自定义的<leader>值，因此我们还需要在.vimrc文件中设置<leader>。比如将其设置为“,”，在.vimrc中加入下面的代码：

> let mapleader=","

为了激活<backspace>键的回删功能，可以在.vimrc中加入：

> set backspace=2

为了在编辑框左侧显示行数，可以在.vimrc中加入：

> set number

python-mode的详细命令可以查看它的文档，这里列出几条常用的：

> 自动补全： \<C-n\>或者\<C-p\> （\<C-n\>表示同时按下ctr键和n键）

> 跳转到函数定义处：\<C-]\>

> 运行代码：\<leader\>r

> 重构：\<C-c\>rr