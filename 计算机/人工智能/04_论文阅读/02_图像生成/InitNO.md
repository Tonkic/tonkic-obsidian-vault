### 目标
优化diffusion模型的初始噪声
##### 两个量化指标
1. **交叉注意力响应分数 (Cross-attention response score, $\mathcal{S}_{CrossAttn}$)**：
- 计算中间特征（送入diffusion之后的第一步去噪之后的特征图）**与**输入文本提示词（Text Tokens）之间的相关性
1. **自注意力冲突分数 (Self-attention conflict score, $\mathcal{S}_{SelfAttn}$)**：
- 自注意力图，其空间分辨率是 $16 \times 16$，每个像素和其他像素进行self-attention（Stable Diffusion 内部的 U-Net 网络进行计算）