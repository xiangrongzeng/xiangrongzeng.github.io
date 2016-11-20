---
layout: post  
title: 强化学习及深度强化学习图解
categories: [reinforcement learning]  
tags: [reinforcement learning]  
description: 「图解强化学习」   
---

本文简要介绍强化学习的基本内容（图写的比较详细，后面可能会再补充一些文字）

# Q-learning
学习过程
<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/q-learning-learn.jpg" align="center"></p>
</center>

使用过程
<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/q-learning-use.jpg" align="center"></p>
</center>

# DQN
## Overview
从状态s选择动作$$a_k$$，再获得奖励$$r_(s,a_k)$$和下一个状态$$s'$$，整个过程可以用元组$$(s,a_k,r_(s,a_k),s')$$来记录。
<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/dqn-state2action.jpg" align="center"></p>
</center>

<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/dqn-stateaction2rewardnewstate.jpg" align="center"></p>
</center>

## 如何训练Q网络
### 思路

Q网络的输出是每个动作的Q值。而我们希望$$𝑄(𝑠,𝑎_𝑘 )=𝑟_(𝑠,𝑎_𝑘 )+𝛾*maxU_(𝑘=1)^𝑛 {𝑄(𝑠^′,𝑎_𝑘 )}$$。（注意只有在网络已经训练好了之后等式才会成立，我们训练的目标就是让这个等式成立。）
### 做法

$$𝑥:x=𝑠$$

$$y:y=𝑟_(𝑠,𝑎_𝑘 )+𝛾*max∪_(𝑘=1)^𝑛 {𝑄(𝑠^′,𝑎_𝑘 )}$$如果𝑠′是终止状态，那$$𝑦=𝑟_(𝑠,𝑎_𝑘 )$$


$$𝑙𝑜𝑠𝑠:𝐿=(y−𝑄(𝑠,𝑎_𝑘 ))^2$$

### 技巧
- 每对一个状态生成元组$$(s,a_k,r_(s,a_k),s')$$后，将其保存到memory中
- 然后从memory随机抽取batch大小个元组作为训练样本
- 训练网络
