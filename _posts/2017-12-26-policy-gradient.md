---
layout: post  
title: PolicyGradient
categories: [rl]  
tags: [rl]  
description: [介绍PolicyGradient算法]   
---
本文根据李宏毅老师[视频](https://www.youtube.com/watch?v=pbQ4qe8EwLo&list=PLJV_el3uVTsPMxPbjeX7PicgWbY7F8wW9&index=17)整理而来。

# 原理

我们的目标是最大化期望的奖赏值$$\hat{R}$$。如果整个模型所有的参数用$$\theta$$来表示，那么我们训练的目标就是得到$$\theta*=argmax_{\theta}\hat{R_{\theta}}$$。

期望的奖赏值$$\hat{R}$$可以写成下面的公式

$$
\hat{R_{\theta}} = \Sigma_sP(s)\Sigma_xR(x,s)P_{\theta}(x|s)
$$





