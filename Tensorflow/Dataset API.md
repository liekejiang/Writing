本篇来自于[CSDN](https://blog.csdn.net/kwame211/article/details/78579035/)

Dataset API 是TF1.3版本引入的一个用于数据读取的新模块。在此之前，在TensorFlow中读取数据一般有两种方法：
* 使用placeholder读内存中的数据
* 使用queue读硬盘中的数据

Dataset现在位于tensorflow的data包里面：` tensorflow.data.Dataset`

### Dataset 与 Iterator

Dataset API里面有两个最基础的class：`Dataset`&`Iterator`。其中Dataset可以看作是相同类型元素的有序列表，这里的元素可以是vector也可以是string image甚至是tuple或dict。
```python
import tensorflow as tf
import numpy as np
 
dataset = tf.data.Dataset.from_tensor_slices(np.array([1.0, 2.0, 3.0, 4.0, 5.0]))
```
上面的代码创建一个dataset，其中含有5个元素。那么如何将这些元素取出呢？ 方法是从`Dataset`中实例化一个`Iterator`然后进行迭代，这里我们就明白了`Dataset`与`Iterator`的关系。

---
在非`Eager`模式下，读取上述元素的方法是：
```python
iterator = dataset.make_one_shot_iterator() ##one_shot 这里代表只能从头到尾读一次
one_element = iterator.get_next()  
with tf.Session() as sess:
    for i in range(5):
        print(sess.run(one_element))
```
上面创建的`Dataset`里面的元素将会被一一打印出来。由于这是非`Eager`模式，所以`one_element`只是一个Tensor，并不是一个实际的值。调用`sess.run(one_element)`后，才能真正地取出一个值。如果一个dataset中元素被读取完了，再尝试`sess.run(one_element)`的话，就会抛出`tf.errors.OutOfRangeError`异常，这个行为与使用队列方式读取数据的行为是一致的。在实际程序中，可以在外界捕捉这个异常以判断数据是否读取完，请参考下面的代码：
```python
dataset = tf.data.Dataset.from_tensor_slices(np.array([1.0, 2.0, 3.0, 4.0, 5.0]))
iterator = dataset.make_one_shot_iterator()
one_element = iterator.get_next()
with tf.Session() as sess:
    try:
        while True:
            print(sess.run(one_element))
    except tf.errors.OutOfRangeError:
        print("end!")
```

---
在`Eager`模式下，创建Iterator的方法有所不同。是通过tfe.Iterator(dataset)的形式直接创建Iterator并迭代。迭代时可以直接取出值，不需要使用sess.run()：
```python
import tensorflow.contrib.eager as tfe
tfe.enable_eager_execution()
 
dataset = tf.data.Dataset.from_tensor_slices(np.array([1.0, 2.0, 3.0, 4.0, 5.0]))
 
for one_element in tfe.Iterator(dataset):
    print(one_element)
```

### 进阶
之前我们使用了
`dataset = tf.data.Dataset.from_tensor_slices(np.array([1.0, 2.0, 3.0, 4.0, 5.0]))`
创建了一个最简单的Dataset。但这个函数的真正作用是切分并且传入Tensor的第一个维度，生成相应的dataset：
`dataset = tf.data.Dataset.from_tensor_slices(np.random.uniform(size=(5, 2)))`
对于这行代码来说，生成的矩阵是5*2的，对第一维进行切分会生成5个长度为2的向量。而在实际应用中，我们还可以希望`Dataset`中的每个元素具有更复杂的形式，如每个元素是一个Python中的元祖，或是Python中的词典。例如，在图象识别中，一个元素可以是{“image”: image_tensor, “label”: label_tensor}的形式，这样处理起来更方便。这种格式的创建方法如下：
```python
dataset = tf.data.Dataset.from_tensor_slices(
    {
        "a": np.array([1.0, 2.0, 3.0, 4.0, 5.0]),                                       
        "b": np.random.uniform(size=(5, 2))
    }
)
```
这时函数会分别切分`a`和`b`中的数值，最终元素就是类似于{“a”: 1.0, “b”: [0.9, 0.1]}的形式。

### 对元素进行变换
Dataset支持一类特殊的操作：Transformation。一个Dataset可以通过Transformation变成一个新的Dataset。Transformation操作通常包括数据变换，打乱，扭曲，组成batch和epoch等操作。常用的有：
* map
* batch
* shuffle
* repeat

##### 1.MAP
map接收一个函数，Dataset中的每个元素都会被当作这个函数的输入，并将函数返回值作为新的Dataset，如果我们可以对dataset中每个元素的值加1:
```python
dataset = tf.data.Dataset.from_tensor_slices(np.array([1.0, 2.0, 3.0, 4.0, 5.0]))
dataset = dataset.map(lambda x: x + 1) # 2.0, 3.0, 4.0, 5.0, 6.0
```

##### 2. Batch
batch就是将多个元素组合成batch，如下面的程序将dataset中的每个元素组成了大小为32的batch：
`dataset = dataset.batch(32)`

##### 3. shuffle
shuffle的功能是打乱dataset中的元素，其参数buffersize，表示打乱时用的buffer的大小:
`dataset = dataset.shuffle(buffer_size = 1000)`

##### 4. repeat
repeat的功能就是将整个序列重复很多次，主要用来处理机器学习中的epoch，假设原来的数据是一个epoch，使用repeat(5)就可以将之变成5个epoch：`dataset = dataset.repeat(5)`
如果直接调用repeat()不给参数，生成的序列就会无限重复下去，没有结束，因此也不会抛出tf.errors.OutOfRangeError异常

### 读入磁盘图片于对应的label
如果我们想读入存放在硬盘中的图片和对应的label，将其打乱组成batch_size = 32的训练样本，epoch=10:
```python
# 函数的功能是将filename对应的图片文件读进来，并缩放到统一的大小
def _parse_function(filename, label):
  image_string = tf.read_file(filename)
  image_decoded = tf.image.decode_image(image_string)
  image_resized = tf.image.resize_images(image_decoded, [28, 28])
  return image_resized, label
 
# 图片文件的列表
filenames = tf.constant(["/var/data/image1.jpg", "/var/data/image2.jpg", ...])
# label[i]就是图片filenames[i]的label
labels = tf.constant([0, 37, ...])
 
# 此时dataset中的一个元素是(filename, label)
dataset = tf.data.Dataset.from_tensor_slices((filenames, labels))
 
# 此时dataset中的一个元素是(image_resized, label)
dataset = dataset.map(_parse_function)
 
# 此时dataset中的一个元素是(image_resized_batch, label_batch)
dataset = dataset.shuffle(buffersize=1000).batch(32).repeat(10)
```
在这个过程中，dataset经历三次转变：
1. 运行`dataset = tf.data.Dataset.from_tensor_slices((filenames, labels))`后，dataset的一个元素是(filename, label)。filename是图片的文件名，label是图片对应的标签。
2. 之后通过map，将filename对应的图片读入，并缩放为28x28的大小。此时dataset中的一个元素是(image_resized, label)
3. 最后，`dataset.shuffle(buffersize=1000).batch(32).repeat(10)`的功能是：在每个epoch内将图片打乱组成大小为32的batch，并重复10次。最终，dataset中的一个元素是(image_resized_batch, label_batch)，image_resized_batch的形状为(32, 28, 28, 3)，而label_batch的形状为(32, )，接下来我们就可以用这两个Tensor来建立模型了。

### Dataset的其他创建方法
除了`tf.data.Dataset.from_tensor_slices`外，目前Dataset API还提供了另外三种创建Dataset方法：
* `tf.data.TextLineDataset()`：这个函数的输入是一个文件的列表，输出是一个dataset。dataset中的每一个元素就对应了文件中的一行。可以使用这个函数来读入CSV文件。
* `tf.data.FixedLengthRecordDataset()`：这个函数的输入是一个文件的列表和一个record_bytes，之后dataset的每一个元素就是文件中固定字节数record_bytes的内容。通常用来读取以二进制形式保存的文件，如CIFAR10数据集就是这种形式。
* `tf.data.TFRecordDataset()`：顾名思义，这个函数是用来读TFRecord文件的，dataset中的每一个元素就是一个TFExample。

### 更多类型的Iterator
在非Eager模式下，最简单的创建Iterator的方法就是通过dataset.make_one_shot_iterator()来创建一个one shot iterator。除了这种one shot iterator外，还有三个更复杂的Iterator，即：

* initializable iterator
* reinitializable iterator
* feedable iterator

##### 1. initializable iterator
`initializable`必须要在使用前通过`sess.run()`来初始化，可以将placeholder带入iterator中，可以方便我们通过快速定义新的iterator。一个简单的`initializable`实例：
```python
limit = tf.placeholder(dtype=tf.int32, shape=[])
dataset = tf.data.Dataset.from_tensor_slices(tf.range(start=0, limit=limit))
iterator = dataset.make_initializable_iterator()
next_element = iterator.get_next()
 
with tf.Session() as sess:
    sess.run(iterator.initializer, feed_dict={limit: 10})
    for i in range(10):
      value = sess.run(next_element)
      assert i == value
```
此时的limit相当于一个“参数”，它规定了Dataset中数的“上限”。initializable iterator还有一个功能：**读入较大的数组**。在使用tf.data.Dataset.from_tensor_slices(array)时，实际上发生的事情是将array作为一个tf.constants保存到了计算图中。当array很大时，会导致计算图变得很大，给传输、保存带来不便。这时，我们可以用一个placeholder取代这里的array，并使用initializable iterator，只在需要时将array传进去，这样就可以避免把大数组保存在图里，示例代码为（来自官方例程）：
```python
# 从硬盘中读入两个Numpy数组
with np.load("/var/data/training_data.npy") as data:
  features = data["features"]
  labels = data["labels"]
 
features_placeholder = tf.placeholder(features.dtype, features.shape)
labels_placeholder = tf.placeholder(labels.dtype, labels.shape)
 
dataset = tf.data.Dataset.from_tensor_slices((features_placeholder, labels_placeholder))
iterator = dataset.make_initializable_iterator()
sess.run(iterator.initializer, feed_dict={features_placeholder: features,
                                          labels_placeholder: labels})
```


