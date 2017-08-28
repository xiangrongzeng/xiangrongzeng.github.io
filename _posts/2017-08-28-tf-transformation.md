---
layout: post  
title: Tensorflow矩阵变形  
categories: [blog ]  
tags: [blog, ]  
description: 「矩阵变换的相关操作」   
---
[官方API](https://www.tensorflow.org/versions/r1.2/api_guides/python/array_ops)

## tf.reshape

	# tensor 't' is [1, 2, 3, 4, 5, 6, 7, 8, 9]
	# tensor 't' has shape [9]
	reshape(t, [3, 3]) ==> [[1, 2, 3],
	                        [4, 5, 6],
	                        [7, 8, 9]]
	
	# tensor 't' is [[[1, 1], [2, 2]],
	#                [[3, 3], [4, 4]]]
	# tensor 't' has shape [2, 2, 2]
	reshape(t, [2, 4]) ==> [[1, 1, 2, 2],
	                        [3, 3, 4, 4]]
	
	# tensor 't' is [[[1, 1, 1],
	#                 [2, 2, 2]],
	#                [[3, 3, 3],
	#                 [4, 4, 4]],
	#                [[5, 5, 5],
	#                 [6, 6, 6]]]
	# tensor 't' has shape [3, 2, 3]
	# pass '[-1]' to flatten 't'
	reshape(t, [-1]) ==> [1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4, 5, 5, 5, 6, 6, 6]
	
	# -1 can also be used to infer the shape
	
	# -1 is inferred to be 9:
	reshape(t, [2, -1]) ==> [[1, 1, 1, 2, 2, 2, 3, 3, 3],
	                         [4, 4, 4, 5, 5, 5, 6, 6, 6]]
	# -1 is inferred to be 2:
	reshape(t, [-1, 9]) ==> [[1, 1, 1, 2, 2, 2, 3, 3, 3],
	                         [4, 4, 4, 5, 5, 5, 6, 6, 6]]
	# -1 is inferred to be 3:
	reshape(t, [ 2, -1, 3]) ==> [[[1, 1, 1],
	                              [2, 2, 2],
	                              [3, 3, 3]],
	                             [[4, 4, 4],
	                              [5, 5, 5],
	                              [6, 6, 6]]]
	
	# tensor 't' is [7]
	# shape `[]` reshapes to a scalar
	reshape(t, []) ==> 7	


## tf.squeeze
	# 't' is a tensor of shape [1, 2, 1, 3, 1, 1]
	shape(squeeze(t)) ==> [2, 3]
	
	# 't' is a tensor of shape [1, 2, 1, 3, 1, 1]
	shape(squeeze(t, [2, 4])) ==> [1, 2, 3, 1]


## tf.expand_dims
	# 't' is a tensor of shape [2]
	shape(expand_dims(t, 0)) ==> [1, 2]
	shape(expand_dims(t, 1)) ==> [2, 1]
	shape(expand_dims(t, -1)) ==> [2, 1]
	
	# 't2' is a tensor of shape [2, 3, 5]
	shape(expand_dims(t2, 0)) ==> [1, 2, 3, 5]
	shape(expand_dims(t2, 2)) ==> [2, 3, 1, 5]
	shape(expand_dims(t2, 3)) ==> [2, 3, 5, 1]

## tf.concat
	t1 = [[1, 2, 3], [4, 5, 6]]
	t2 = [[7, 8, 9], [10, 11, 12]]
	tf.concat([t1, t2], 0) ==> [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]]
	tf.concat([t1, t2], 1) ==> [[1, 2, 3, 7, 8, 9], [4, 5, 6, 10, 11, 12]]

## tf.stack
	# 'x' is [1, 4]
	# 'y' is [2, 5]
	# 'z' is [3, 6]
	stack([x, y, z]) => [[1, 4], [2, 5], [3, 6]]  # Pack along first dim.
	stack([x, y, z], axis=1) => [[1, 2, 3], [4, 5, 6]]


## tf.unstack
	# 'x' is [[1, 4], [2, 5], [3, 6]]
	unstack(y, axis=0) => [a, b, c]
	a is [1, 4]
	b is [2, 5]
	c is [3, 6]

	unstack(y, axis=1) => [a, b]
	a is [1, 2, 3]
	b is [4, 5, 6]

## tf.transpose

	# 'x' is [[1 2 3]
	#         [4 5 6]]
	tf.transpose(x) ==> [[1 4]
	                     [2 5]
	                     [3 6]]
	
	# Equivalently
	tf.transpose(x, perm=[1, 0]) ==> [[1 4]
	                                  [2 5]
	                                  [3 6]]
	
	# 'perm' is more useful for n-dimensional tensors, for n > 2
	# 'x' is   [[[1  2  3]
	#            [4  5  6]]
	#           [[7  8  9]
	#            [10 11 12]]]
	# Take the transpose of the matrices in dimension-0
	tf.transpose(x, perm=[0, 2, 1]) ==> [[[1  4]
	                                      [2  5]
	                                      [3  6]]
	
	                                     [[7 10]
	                                      [8 11]
	                                      [9 12]]]
	
## tf.one_hot
	one_hot([0, 1, 2], depth=3) => [[1., 0., 0.],
								   [0., 1., 0.],
								   [0., 0., 1.]]
								   
	one_hot([0, 2, -1, 1], depth=3, on_value=5.0, off_value=0.0) => [5.0 0.0 0.0]  // one_hot(0)
  									   								[0.0 0.0 5.0]  // one_hot(2)
  									   								[0.0 0.0 0.0]  // one_hot(-1)
  									   								[0.0 5.0 0.0]  // one_hot(1)

	one_hot([[0, 2], [1, -1], depth=3, on_value=1.0, off_value=0.0) => [
																		    [1.0, 0.0, 0.0]  // one_hot(0)
																		    [0.0, 0.0, 1.0]  // one_hot(2)
																		][
																		    [0.0, 1.0, 0.0]  // one_hot(1)
																		    [0.0, 0.0, 0.0]  // one_hot(-1)
																		]


