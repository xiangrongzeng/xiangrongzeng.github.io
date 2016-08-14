---
layout: post
title: TensorFlow如何保存和加载变量
categories: [TensorFlow ]  
tags: [TensorFlow]  
description: 「TensorFlow保存、加载变量」   
---

本文介绍在TensorFlow中如何保存和加载变量。本文参考[官网教程](https://www.tensorflow.org/versions/r0.10/how_tos/variables/index.html)。

## 前言
TensorFlow中，不论是保存还是加载已经保存好的变量值，都是通过tf.train.Saver()来进行，并且非常简单易用。

## 保存变量
先给出示例代码：

```python
# Create some variables.
v1 = tf.Variable(..., name="v1")
v2 = tf.Variable(..., name="v2")
...
# Add an op to initialize the variables.
init_op = tf.initialize_all_variables()

# Add ops to save and restore all the variables.
saver = tf.train.Saver()

# Later, launch the model, initialize the variables, do some work, save the
# variables to disk.
with tf.Session() as sess:
  sess.run(init_op)
  # Do some work with the model.
  ..
  # Save the variables to disk.
  save_path = saver.save(sess, "/tmp/model.ckpt")
  print("Model saved in file: %s" % save_path)
```

这种情况下，我们保存了所有的变量。

## 加载已经保存的变量
如果我们要从文件中加载一个变量，那么这个变量就无需初始化了。

```python
# Create some variables.
v1 = tf.Variable(..., name="v1")
v2 = tf.Variable(..., name="v2")
...
# Add ops to save and restore all the variables.
saver = tf.train.Saver()

# Later, launch the model, use the saver to restore variables from disk, and
# do some work with the model.
with tf.Session() as sess:
  # Restore variables from disk.
  saver.restore(sess, "/tmp/model.ckpt")
  print("Model restored.")
  # Do some work with the model
  ...
```

