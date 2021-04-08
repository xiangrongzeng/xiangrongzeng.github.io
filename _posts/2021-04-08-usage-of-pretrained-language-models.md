---
layout: post  
title: 如何在实际生产中使用预训练语言模型  
categories: [blog ]  
tags: [blog, ]  
description: 「如何在实际生产中使用预训练语言模型」   
---

以BERT为代表的预训练语言模型比“传统”的深度学习模型效果有明显的提升，并且BERT模型可以很容易的支持诸如分类、序列标注等常用NLP任务。
<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/teacher_training.jpg" width="3000" align="center"></p>
</center>
但是预训练语言模型通常包含了数量巨大的参数，使得模型在实际生产环境中难以满足实时率的要求。
我们在实践中采用模型蒸馏的方式来克服模型参数过大导致的问题。
模型蒸馏时，student模型的性能通常会比teacher模型的性能有所下降。我们在实践中发现，蒸馏过程中引入大量无标签数据可以让student模型的性能尽可能接近（甚至超过）teacher模型的性能，从而实现“鱼与熊掌兼得”。
<center>
	<p><img src="https://raw.githubusercontent.com/xiangrongzeng/xiangrongzeng.github.io/master/_posts/graph/distilling.jpg" width="3000" align="center"></p>
</center>
