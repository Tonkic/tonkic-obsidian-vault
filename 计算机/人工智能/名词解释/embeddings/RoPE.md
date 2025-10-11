- Rotary Position Embedding
- 目前用的最多的方法
- 相对位置编码
- 内积运算不受相对旋转影响
![](../../../../pic/Pasted%20image%2020250924234253.png)
- 高维向量旋转的实现：
	- 切成二维的形状
	- 每两个维度都将以某个$\theta$角进行旋转
	- ![](../../../../pic/Pasted%20image%2020250924234820.png)
- $$
f_{\{q,k\}}(\mathbf{x}_m, m) = \mathbf{R}_{\Theta,m}^d \mathbf{W}_{\{q,k\}} \mathbf{x}_m
$$

```python
# 先给出绝对位置编码(0,max_seq_len-1)
positions = torch.arange(self.max_seq_len)

```