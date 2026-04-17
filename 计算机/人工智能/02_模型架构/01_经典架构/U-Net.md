---
tags:
  - 计算机
  - 人工智能
  - 模型架构
  - 经典架构
---
Encoder-Decoder 结构
![Pasted image 20251126150613](../../../../pic/Pasted%20image%2020251126150613.png)

##### Skip Connection
- 在 Decoder 进行上采样（Upsampling）后，特征图的大小会变大，变得和 Encoder 对应层的特征图一样大。此时，U-Net 会把 Encoder 那一层的特征图“拉过来”，和 Decoder 当前的特征图在 Channel（通道）维度上拼在一起。
- 公式：
$$Output = \text{Concat}([D, E], \text{dim=channel})$$
    
- 拼接后的新特征图形状变为 $2C \times H \times W$