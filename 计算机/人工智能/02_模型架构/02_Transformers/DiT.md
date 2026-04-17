---
tags:
  - 计算机
  - 人工智能
  - 模型架构
  - Transformers
---
---
aliases:
  - Diffusion Transformer
---
Diffusion Transformer的核心思想是使用Transformer作为扩散模型的骨干网络，而不是传统的卷积神经网络(如U-Net)，以处理图像的潜在表示。
### 架构图
![Pasted image 20251101221243](../../../../pic/Pasted%20image%2020251101221243.png)
DiT的核心组件：DiT有三种变种形式，分别与In-Context Conditioning、Cross-Attention、adaLN-Zero相组合。