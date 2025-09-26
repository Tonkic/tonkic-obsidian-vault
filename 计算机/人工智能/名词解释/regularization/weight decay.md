---
aliases:
  - L2 正则
  - L2 regularization
---


l2 是 先加入正则项 loss 然后计算梯度更新量；

weight decay是先 计算梯度更新量 然后简单的进行权重衰减，即简单地对更新后地权重进行 (1-weight_decay_ratio)的 放缩。

二者在sgd下等价，在其它复杂的optimzier下不等价。