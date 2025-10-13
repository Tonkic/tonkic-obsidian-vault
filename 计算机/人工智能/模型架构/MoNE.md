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

- 输入向量input兵分两路，左边up右边gate，逐元素相乘后down，可以理解为FFN的两层MLP中间插入了一个gate
### METHOD
![](../../../pic/Pasted%20image%2020251012200351.png)
- x: 当前层的输入向量
- n: 可供选择的专家总数
- $K_E$: 需要从 n 个专家中选出的专家数量
- $K_N$: 在每个被选中的专家内部，需要选出的神经元数量
- Act: 用于处理专家分数的激活函数（如 Softmax）
- 7.后才与MoE不同，$G_i$ 表示神经元激活值，第9步根据topK $G_i$ 取abs绝对值得出要选哪些神经元
- 如何修改专家的参数？这里使用 $W_{up}$ 升维后 $W_{gate}$ 修改再使用 $W_{down}$ 降维为原来的维度



#### 公式

$$
E_i(\mathbf{x}) = \mathbf{W}_{\text{down}}^i(\text{SiLU}(\mathbf{W}_{\text{gate}}^i\mathbf{x}) \odot \mathbf{W}_{\text{up}}^i\mathbf{x})
$$

- 专家E_i的表达式

$$
\mathbf{P}(\mathbf{x}) = \text{Act}(\text{topK}(\text{Router}(\mathbf{x})))
$$

- Act是激活函数

$$
\text{MoE}(\mathbf{x}) = \sum_{i=1}^{N_E} \mathbf{P}(\mathbf{x})_i E_i(\mathbf{x})
$$

- 通过P(x)限制激活的专家，1激活0不激活

$$
\mathcal{L}_{\text{aux}} = \alpha_{\text{aux}} \cdot N_E \cdot \sum_{i=1}^{N_E} f_i \cdot P_i, \quad \text{where}
$$


$$
f_i = \frac{1}{T} \sum_{\mathbf{x} \in \mathcal{B}} \mathbb{I}\{i \in \text{argtopK}(\text{Router}(\mathbf{x}))\}, \quad P_i = \frac{1}{T} \sum_{\mathbf{x} \in \mathcal{B}} \text{Act}(\text{topK}(\text{Router}(\mathbf{x})))[i]
$$

- banlance loss，

$$
\mathbf{G} = \text{SiLU}(\mathbf{W}_{\text{gate}}^i\mathbf{x}) \in \mathbb{R}^{d_{\text{expert}}}, \quad \mathbf{H} = \mathbf{W}_{\text{up}}^i\mathbf{x} \in \mathbb{R}^{d_{\text{expert}}}
$$



$$
E_i(\mathbf{x}) = \mathbf{W}_{\text{down}}^i(\mathbf{G} \odot \mathbf{H})
$$

$$
E_i(\mathbf{x}) = \sum_{k=1}^{d_{\text{expert}}} \mathbf{W}_{\text{down}}^i[:, k](G[k] \cdot H[k])
$$

$$
= \sum_{k=1}^{d_{\text{expert}}} G[k] \cdot (\mathbf{W}_{\text{down}}^i[:, k](\mathbf{W}_{\text{up}}^i[k, :]\mathbf{x}))
$$

- 后面 $(\mathbf{W}_{\text{down}}^i[:, k](\mathbf{W}_{\text{up}}^i[k, :]\mathbf{x})$ 就是 $A_K$

$$
E_i(\mathbf{x}) = \sum_{k=1}^{d_{\text{expert}}} G[k] \cdot A_k \mathbf{x}, \quad \text{where } A_k = \mathbf{W}_{\text{down}}^i[:, k]\mathbf{W}_{\text{up}}^i[k, :] \in \mathbb{R}^{d_{\text{model}} \times d_{\text{model}}}
$$

- E_i分解为A_i