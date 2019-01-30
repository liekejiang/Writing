# Loss Function in TF

When using TF, we could find that there are many different **Loss Function** such as :

* [tf.nn.sigmoid_cross_entropy_with_logits](https://www.tensorflow.org/versions/r1.4/api_docs/python/tf/nn/sigmoid_cross_entropy_with_logits)
* [tf.nn.softmax_cross_entropy_with_logits](https://www.tensorflow.org/versions/r1.4/api_docs/python/tf/nn/softmax_cross_entropy_with_logits)
* [tf.nn.sparse_softmax_cross_entropy_with_logits](https://www.tensorflow.org/versions/r1.4/api_docs/python/tf/nn/sparse_softmax_cross_entropy_with_logits)

First introduce logit function:  L( p)= ln $$\frac{p}{1-p}\quad$$. We could map the input from [0,1] to the whole Real domain。
