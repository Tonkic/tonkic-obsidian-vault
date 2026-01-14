### 实现
$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}} + \text{mask}\right)V$$

加一个mask矩阵，屏蔽的区域设置为-inf
[softmax](../激活函数/Softmax.md) -inf为0，代表其最终输出的logits为0