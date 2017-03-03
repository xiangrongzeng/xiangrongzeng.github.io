---
layout: post  
title: 用PyCharm专业版实现远程调试、运行代码  
categories: [Tools]  
tags: [Tools]  
description: 「用PyCharm专业版实现远程调试、运行代码」   
---

本文介绍如何使用用PyCharm专业版实现远程调试、运行代码。

## 需求
我们进行深度学习程序开发的时候几乎必然会用到深度学习的库，比如TensorFlow、Theano等。通常我们用专门的服务器来运行程序，这些服务器都在远程，且常常缺乏IDE，因此我们通常是在本地编写好程序再放到服务器上去运行，很不方便，尤其是要调试程序的时候。如果能够再本地编写程序，然后自动同步到服务器上，并且还能够再本地对其进行远程调试和运行，那就方便多了。

接下来就介绍如何用PyCharm专业版实现远程调试、运行代码。

## 过程
### 获取PyCharm专业版
PyCharm专业版提供了我们需要的工具。它可以从[这里](https://www.jetbrains.com/pycharm/download/#section=windows)下载。

需要注意的是，专业版是收费的。但是如果你是在校学生或者老师的话，是可以免费使用JetBrains旗下的所有专业版IDE的。这里要赞一下良心企业[JetBrains](https://www.jetbrains.com/)。

学生或者老师免费获取license的方法：

- 首先到[官网](https://www.jetbrains.com/)注册一个账户，注册时注意用你的学校的邮箱（中科院的邮箱也可以）。
- 在[这里](https://www.jetbrains.com/shop/eform/students)填写你的相关信息。注意邮箱要用学校邮箱。
- 然后就是等邮件了，一般几分钟就能收到邮件（注意检查垃圾邮件箱）。然后跟着邮件的指示来就可以了。

### 如何设置
打开专业版PyCharm，会要求通过登录或者填写license的方法来验证身份。有时候登录会有问题（可能时国内网络的问题），这时可以选择填写license的方法来验证。license可以在[这里](https://account.jetbrains.com/licenses/assets)找到。

接下来我们就进入了IDE。我们需要设置两个地方：远程服务器和远程解析器。

设置远程服务器：Tools -> Deployment -> Configuration 打开设置界面，点击“+”号添加远程主机，如下图所示。最后是选择要同步的文件夹的路径。就是选择你本地需要同步的文件夹和远程服务器需要同步的文件夹，此项在下图中的Mapping中设置。
<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/pyc-add-server-1.jpg" align="center"></p>
</center>
name就是主机的名字（不是ip，可以是一个好记的名字），Type要选择SFTP。然后点确定。接下来填写服务器ip、用户名、密码等，如图：
<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/pyc-add-server-2.jpg" align="center"></p>
</center>

接下来讲讲如何设置远程解析器：File -> Settings -> Project: -> Project Interpreter。如图所示：
<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/pyc-add-remote-interpreter.jpg" align="center"></p>
</center>
接下来一路确定就可以了。

这样就配置好了，已经可以远程调试、运行TensorFlow的程序啦~

