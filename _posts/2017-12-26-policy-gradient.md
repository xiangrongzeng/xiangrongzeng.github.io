---
layout: post  
title: PolicyGradient
categories: [rl]  
tags: [rl]  
description: [介绍PolicyGradient算法]   
---
本文根据李宏毅老师[视频](https://www.youtube.com/watch?v=pbQ4qe8EwLo&list=PLJV_el3uVTsPMxPbjeX7PicgWbY7F8wW9&index=17)整理而来。

# 原理

我们的目标是最大化期望的奖赏值$$\hat{R}$$。如果整个模型所有的参数用$$\theta$$来表示，那么我们训练的目标就是得到$$\theta^*=argmax_{\theta}\hat{R_{\theta}}$$。
期望的奖赏值$$\hat{R}$$可以写成下面的公式：

$$
\hat{R_{\theta}} = \sum_sP(s)\sum_xR(x,s)P_{\theta}(x|s)
$$

解释一下这个公式：$$s$$表示某个可能的状态，$$P(s)$$表示的是这个状态出现的概率；$$x$$表示某个可能的动作，$$P_{\theta}(x|s)$$表示在给定状态$$s$$的情况下，参数是$$\theta$$的这个模型选择动作$$x$$的概率；$$R(x,s)$$表示的是在状态$$s$$下执行了动作$$x$$之后得到的奖赏值。
这个公式其实就是奖赏值的期望的公式：

$$
\hat{R_{\theta}} = \sum_sP(s)\sum_xR(x,s)P_{\theta}(x|s)
$$
$$
=E_{s\sim P(s)}[E_{x\sim P_{\theta}(x|s)}R(x,s)]
$$
$$
=E_{s\sim P(s),x\sim P_{\theta}(x|s)}R(x,s)
$$

因为要最大化$$\hat{R_{\theta}}$$，我们可以用梯度上升的办法来更新$$\theta$$值，因此需要用到$$\hat{R_{\theta}}$$的梯度。我们对上式进行求导：

$$
\nabla\hat{R_{\theta}} = \sum_sP(s)\sum_xR(x,s)\nabla P_{\theta}(x|s)
$$
$$
=\sum_sP(s)\sum_xR(x,s)P_{\theta}(x|s)\frac{\nabla P_{\theta}(x|s)}{P_{\theta}(x|s)}
$$
$$
=\sum_sP(s)\sum_xR(x,s)P_{\theta}(x|s)\nabla \log P_{\theta}(x|s)
$$
$$
=E_{s\sim P(s),x\sim P_{\theta}(x|s)}R(x,s)\nabla \log P_{\theta}(x|s)
$$

但是在实际操作过程中，我们不可能真正精确的计算奖赏值的期望，因此我们用采样的方法来估计这个期望值，比如我们采样了N个样本：
$$
(x^1,s^1),(x^2,s^2),...,(x^N,s^N)
$$

因此上面奖赏值的期望的导数就可以近似写成：
$$
\nabla\hat{R_{\theta}} \approx \frac{1}{N} \sum_{i=1}^{N} R(x^i,s^i)\nabla \log P_{\theta}(x^i|s^i)
$$

更新参数 $$\theta^{new} \rightarrow \theta^{old}+\eta \nabla\hat{R_{\theta}}$$







