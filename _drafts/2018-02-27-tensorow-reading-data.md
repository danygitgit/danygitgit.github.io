 ---
layout: post
title: Tensorflow 高效读取数据的三种方法
categories: Tensorflow
description: tensorflow如何导入数据？有三种方法
keywords: tensorflow，reading data
---

Tensorflow 读取数据！！

---
#### 方法
将数据导入tensorflow程序中有三种常用的方法：
1. Feeding: 在tensorflow程序运行的每一个步骤，通过Python代码提供导入
2. 文件读取：在tensorflow 图开始之前，利用输入管道读取文件数据
3. 预加载数据：在TensorFlow图中定义常量或变量来保存所有数据（对于小数据集）

资源链接，[参考这里](https://github.com/jikexueyuanwiki/tensorflow-zh/blob/master/SOURCE/how_tos/reading_data/index.md) 

---
#### 目录
1. Feeding
2. Reading from files
     (a) Filenames, shuffling, and epoch limits
     (b) File formats
     (c) Preprocessing
     (d) Batching
     (e) Creating threads to prefetch using QueueRunner objects
     (f) Filtering records or producing multiple examples per record
     (g) Sparse input data
3. Preloaded data
4. Multiple input pipelines

----
##### Feeding
tensorflow的Feed机制可以让你将数据注入到计算图中的任何张量中去，Python计算可以提供数据给图形。
将feed数据通过feed_dict参数提供run()和eval()来启动计算

```
with tf.Session():
  input = tf.placeholder(tf.float32)
  classifier = ...
  print classifier.eval(feed_dict={input: my_python_preprocessing_fn()})
```
注： 当你用feed数据替代任何张量时（包括常量和变量），最佳方式使用 placeholder(占位符)，它的存在仅仅是服务于feed数据，它没有初始化，不包含任何数据，如果执行时没有feed，它将会发生错误，所以记得feed。