通过Softmax函数就可以将多分类的输出值转换为范围在【0, 1】和为1的概率分布。

假设有一个数组V，$V_i$表示V中的第i个元素，那么这个元素的softmax值为
$$\sigma(i) = \frac{e^{i}}{\sum_{j=1}^K e^{j}}$$

tansformer中的QV相乘求相似度的时候，需要做一个缩放防止出现极端情况，如图，分别是hardmax和softmax的结果，
hardmax就是没有指数e，直接就是$X_i$
![../../../pic/Pasted image 20250407044224.png](../../../pic/Pasted%20image%2020250407044224.png)