- Rotary Position Embedding
- 目前用的最多的方法
- 相对位置编码
- 内积运算不受相对旋转影响
![](../../../../../pic/Pasted%20image%2020250924234253.png)
- 高维向量旋转的实现：
	- 切成二维的形状
	- 每两个维度都将以某个$\theta$角进行旋转
	- ![](../../../../../pic/Pasted%20image%2020250924234820.png)
- $$
f_{\{q,k\}}(\mathbf{x}_m, m) = \mathbf{R}_{\Theta,m}^d \mathbf{W}_{\{q,k\}} \mathbf{x}_m
$$
	 - $x_m$:第m个位置上的词
	 - $\mathbf{W}_{\{q,k\}}$:对应q和k的两个权重矩阵
```python
# 先给出绝对位置编码(0,max_seq_len-1)
positions = torch.arange(self.max_seq_len)
#计算频率θi​
freqs = 1.0 / (self.theta **(torch.arange(0, self.d_k, 2).float() / self.d_k))
sinusoids = torch.outer(positions, freqs) #outer是外积，即每个位置都与每个频率相乘 shape: [max_seq_len, d_k//2]
self.register_buffer("cos_cache", sinusoids.cos(), persistent=False) #利用register_buffer表示这是固定的，不需要学习
self.register_buffer("sin_cache", sinusoids.sin(), persistent=False)

def forward(self, x: torch.Tensor, token_positions: torch.Tensor) -> torch.Tensor:
        # 这里的x是输入的稠密向量，token_positions是token的位置信息
        cos = self.cos_cache[token_positions]
        sin = self.sin_cache[token_positions]
        cos = cos.unsqueeze(0) # shape: [1, max_seq_len, d_k//2] 对应 [batch, max_seq_len, d_k//2]
        sin = sin.unsqueeze(0) # shape: [1, max_seq_len, d_k//2] 对应 [batch, max_seq_len, d_k//2]
        #  这里还是分奇偶数写容易理解
        x_part1 = x[..., 0::2]
        x_part2 = x[..., 1::2]
        output1 = x_part1 * cos - x_part2 * sin # 偶数位置乘以cos，奇数位置乘以sin
        output2 = x_part1 * sin + x_part2 * cos # 偶数位置乘以sin，奇数位置乘以cos
        # out = torch.cat([output1, output2], dim=-1) # shape: [batch,  max_seq_len, d_k]
        out = torch.stack([output1, output2], dim=-1)  # [batch, seq_len, d_k//2, 2] #用stack能巧妙的把奇数和偶数交叉在一起，cat就不行
        out = out.flatten(-2)  # [batch, seq_len, d_k]
        return out
```