---
layout: post  
title: 那些年我曾经掉过的坑
categories: [experience ]  
tags: [trap]  
description: 「陷阱」   
---



## TensorFlow

> 不要在大循环中使用sess.run()，这样会导致速度慢很多

## Python

在python中如何实现输出不换行：
```python
# python2
print "something",
# python3
print("something", end="")
```

这样就可以实现在同一行滚动输出：
```python
# python2
print "\r rate %d" % (rate),
# python3
print("\r rate %d" % (rate), end="")
```