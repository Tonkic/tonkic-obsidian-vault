#### 一般用于二分类任务的输出层，将输出变成0-1之间的概率
Sigmoid 函数定义：

$$
\sigma(x) = \frac{1}{1 + e^{-x}}
$$

导数（简化形式）：
- 该性质在反向传播中有用

$$
\sigma'(x) = \sigma(x) \cdot \big(1 - \sigma(x)\big)
$$

导数（展开形式）：

$$
\sigma'(x) = \frac{e^{-x}}{(1 + e^{-x})^2}
$$
