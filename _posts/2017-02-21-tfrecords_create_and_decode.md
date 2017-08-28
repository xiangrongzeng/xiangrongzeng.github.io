---
layout: post  
title: TFRecords指南  
categories: [blog ]  
tags: [blog, ]  
description: 「TFRecords指南」   
---
我们举例说明。

在自然语言处理任务中，我们需要对句子进行分类。

假设我们已经将句子表示成了词ID的形式，并且已经将类标转为了数字。也就是说每个句子表示成了一个k维的向量，其中k就是每个句子所包含的词的个数，这个向量的每一维都是一个整数。比如：

```python
k = 6
s1 = [9, 81, 32, 2874, 320, 3764298]  # ['i','am', 'going', 'to', 'eat', '.']
label1 = 0
s2 = [827, 293, 38, 9, 943, 374]    # ['it', 'is', 'a', 'good', 'day', '!']
label2 = 1
s = [s1, s2]  # s是所有句子的集合
labels = [label1, label2]
```

我们现在要将上述数据保存为tfrecords的格式。注意到，我们总共有N个样本（在这里就是N个句子及其对应的标签），对于每个样本，称其为一个example。保存数据时，我们是一个一个样本保存的。

- 打开一个writer，用于将数据写到文件
- 将数据先转为ndarray（因为我们要用到numpy中ndarray.tostring()这个方法）。这里要注意的是，此时要设置数据类型，在这个例子中，词的个数很多，会出现如s1中"3764298"这样的大的数，因此我们选择np.int32类型（如果确保数据中不会出现太大的整数，可以选择其它数据类型，以减小保存后文件的大小）。
- 用tf.train.Example()创建一个example，并将当前样本数据输入进去。
- 将一个example写入文件。
- 关闭writer

```python
filename = 'data.tfrecords'     # 文件名
writer = tf.python_io.TFRecordWriter(filename)  # 打开一个writer
for sentence, label in zip(s, labels):
    #   将数据转为ndarry，并将数据转为Python bytes
    sentence = np.asanyarray(sentence, dtype=np.int32)
    sent_raw = sentence.tostring()
    label_raw = label.tostring()
    #   创建一个example
    example = tf.train.Example(features=tf.train.Features(feature={
        'sent_raw': _bytes_feature(data_raw),
        'label_raw': _bytes_feature(label_raw)
    }))
    #   将当前example写入文件
    writer.write(example.SerializeToString())
#   所有样本都已经写入文件，关闭writer
writer.close()
```

此时本地已经有一个名为data.tfrecords的文件了。

## 如何读入tfrecords格式文件并解析

上面讲了如何将数据保存为tfrecords格式，现在讲讲如何读入并解析tfrecords格式的文件，并且将其用到构建TensorFlow的图中（比如构建神经网络）。

读入tfrecords文件很简单，分为两步：

- 构建一个文件名队列，其实就是把你要读入的那些文件名输入就行。
- 打开一个TFRecordReader来读文件

```python
#   构建文件名队列
filename_queue = tf.train.string_input_producer([train_filename], num_epochs=10)
#   读入多个文件
#filename_queue = tf.train.string_input_producer([train_filename, valid_file_name, test_filename], num_epochs=10)
#   打开TFRecordReader，并用其来读取filename_queue中的文件
reader = tf.TFRecordReader()
_, serialized_example = reader.read(filename_queue)
```

文件读取完之后需要对其进行解析。注意解析时，我们只需要考虑单个样本是怎么样解析就行，并且其中涉及到的名字（比如这里的'data_raw'和'label_raw'）必须跟构建时一样。

```python
features = tf.parse_single_example(
        serialized_example,
        features={
            'data_raw': tf.FixedLenFeature([], tf.string),
            'label_raw': tf.FixedLenFeature([], tf.string)
        })
#   features['data_raw']的二进制格式的数据，我们需要为其指明需要解码为哪种类型的数据。我们在将原始数据保存为tfrecords格式时使用的是int32类型，因此此处也要用int32类型。
sent = tf.decode_raw(features['data_raw'], tf.int32)
#   在转为tfrecords格式时，会丢失数据的原始shape，因此我们需要在这里对数据进行reshape。
#   这个任务中我们不需要reshape，因为原始数据就是1*k的向量，在其它任务中，比如图像是m*n维的，我们就需要进行reshape操作。
sent.set_shape([1*k])
#sent = tf.reshape(sent, [m, n])
sent = tf.cast(sent, tf.int32)
```

到这里，解析已经结束了。为了直观的感受每一步会产生什么结果，我们可以用sess.run(sent)来查看输出的结果。

我们通常需要以batch的形式来使用数据，TensorFlow提供了方便的函数来完成这个任务。tf.train.shuffle_batch()可以为我们依次产生batch，并且我们可以直接将它的返回值用于构建TensorFlow的图（比如一个神经网络）。

```python
"""tf.train.shuffle_batch()有很多参数，这里只介绍几个常用的。注意参数enqueue_many为Flase（默认值）时，tensors的输入是一个单一的example。如果输入一个shape为[x, y, z]的tensor，函数将会输出shape为[batch_size, x, y, z]的tensor
    batch_size: batch 的大小
    capacity: 整数，队列（queue）中允许的元素个数。它必须大于min_after_dequeue。建议值是min_after_dequeue + (num_threads + a small safety margin) * batch_size
    num_threads: 线程数
    allow_smaller_final_batch:  布尔值，是否允许最后一个batch的size小于batch_size
    min_after_dequeue: 随机选择样本的时候可以用多大的buffer
"""
min_after_dequeue = 10000
capacity = min_after_dequeue + 3 * batch_size
sents = tf.train.shuffle_batch(tensors=[sent, label],
                           batch_size=2,
                           capacity=capacity,
                           num_threads=1,
                           min_after_dequeue=min_after_dequeue)

```

此时的sents可以直接用来构建图了。

将补充完整的代码。

## 参考资料
- http://warmspringwinds.github.io/tensorflow/tf-slim/2016/12/21/tfrecords-guide/
- https://www.tensorflow.org/programmers_guide/reading_data
    - https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/how_tos/reading_data/fully_connected_reader.py
    - https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/how_tos/reading_data/convert_to_records.py

