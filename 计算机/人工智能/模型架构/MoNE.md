### 总结
- MoE选择专家后不一定需要激活全部参数，揭示了在专家内部存在着“第二层稀疏性”
### INTRODUCTION实验图解释
实验Figure1得出只激活一个专家内top0.6的参数，模型的性能几乎没有下降

实验Figure2得出这种稀疏性在模型不同深度的layer中普遍存在
![](../../../pic/Pasted%20image%2020251012202950.png)
Figure3表示传统MoE专家可以分解为神经元粒度FFN的加权求和
$$
E_k(\mathbf{x}) = W_{down}(W_{up}\mathbf{x} \odot \text{SiLU}(W_{gate}\mathbf{x})) = \underbrace{E_i^1 \cdot \text{SiLU}(g_1)}_{\text{score}} + \underbrace{E_i^2 \cdot \text{SiLU}(g_2)}_{\text{score}} + \dots + \underbrace{E_i^n \cdot \text{SiLU}(g_n)}_{\text{score}}
$$
输入向量input兵分两路，左边up右边gate，zu'yuan'su
### METHOD
![](../../../pic/Pasted%20image%2020251012200351.png)
- x: 当前层的输入向量
- n: 可供选择的专家总数
- $K_E$: 需要从 n 个专家中选出的专家数量
- $K_N$: 在每个被选中的专家内部，需要选出的神经元数量
- Act: 用于处理专家分数的激活函数（如 Softmax）
- 7.后才与MoE不同，$G_i$表示神经元激活值，第9步根据topK$G_i$去abs绝对值得出要选哪些神经元
- 如何修改专家的参数？这里使用$W_{up}$升维后修改再使用$W_{down}$降维为原来的维度

#### TODO：为什么Wup保留第k行，而Wdown保留第k列，然后两个相乘得到的就是要保留的神经元k

#### TODO：公式 12推导过程