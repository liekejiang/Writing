# Loss Function in TF

When using TF, we could find that there are many different **Loss Function** such as :

* [tf.nn.sigmoid_cross_entropy_with_logits](https://www.tensorflow.org/versions/r1.4/api_docs/python/tf/nn/sigmoid_cross_entropy_with_logits)
* [tf.nn.softmax_cross_entropy_with_logits](https://www.tensorflow.org/versions/r1.4/api_docs/python/tf/nn/softmax_cross_entropy_with_logits)
* [tf.nn.sparse_softmax_cross_entropy_with_logits](https://www.tensorflow.org/versions/r1.4/api_docs/python/tf/nn/sparse_softmax_cross_entropy_with_logits)

First introduce logit function:  L( p)= ln $$\frac{p}{1-p}\quad$$. We could map the input from [0,1] to the whole Real domain。softmax和sigmoid则都能够将整个实数域映射到[0,1]内。而在TF里，logits指的是该损失函数是在**logit**数值上使用softmax或者sigmoid进行normalization的，期望用户不要将网络输出进行sigmoid或者softmax而是让这些函数在内部进行更高效地运算。

### tf.nn.sigmoid_cross_entropy_with_logits
```python
sigmoid_cross_entropy_with_logits(
    _sentinel=None,
    labels=None,
    logits=None,
    name=None)
```
计算最后一层网络输出的logit和对应的标签label的sigmoid cross entropy loss，衡量独立不互斥离散分类任务的误差，因为这些任务种类和类之间是独立但是不互斥的。举个例子，一张图中可以有多种实例，比如有白猫和黑猫，设多目标检测任务一共有五个分类，label的值可以为[1,1,0,0,0]。 这种任务不需要one hot编码，最后一层输出即是该损失函数loss的输入。
剖开函数内部，因为labels和logits的形状都是[batch_size, num_classes]，那么如何计算他们的交叉熵呢，毕竟它们都不是有效的概率分布（一个batch内输出结果经过sigmoid后和不为1）。其实loss的计算是element-wise的，方法返回的loss的形状和labels是相同的，也是[batch_size, num_classes]，再调用reduce_mean方法计算batch内的平均loss。所以这里的cross entropy其实是一种class-wise的cross entropy，每一个class是否存在都是一个事件，对每一个事件都求cross entropy loss，再对所有的求平均，作为最终的loss。

### tf.nn.softmax_cross_entropy_with_logits
```python
softmax_cross_entropy_with_logits(
    _sentinel=None,
    labels=None,
    logits=None,
    dim=-1,
    name=None)
```
最后一次网络输出logits和labels的softmax cross entropy loss，衡量独立互斥理散分类任务的误差，在这些任务种类与类之间独立且互斥，以图片分类为例，一张图只能属于一个类别。 这种分类就可以使用one-hot编码，与之前的sigmoid相比，区别就是输出每一类的概率和是否为1。label的格式为[batch_size, class_num]。

### tf.nn.sparse_softmax_cross_entropy_with_logits
这个版本是上一个softmax的简易版本，这个版本的logits格式依然是[batch_size, num_class]，但是labels的格式是[batch_size, 1]，每个label的取值是从[0,num_class]的离散值。和上一个softmax的最大区别就是label的格式，一个是one_hot，一个不是。要根据label的格式选择合适的损失函数。