---
tags:
  - 计算机
  - 人工智能
  - 论文阅读
  - 大语言模型
---
### 动机
训练一个模型，使用latent space的token开完成CoT的过程

### 符号
### 1. 文本与序列相关符号
- **$Q$**：输入的**问题（Question）**。
- **$T$**：完整的**显式推理序列（Explicit Reasoning Sequence）**，即思维链（CoT）
- **$S_i$**：显式推理的**子片段（Subsegments）** 3。论文通过一个分割函数 $Seg(\cdot)$ 将长推理链 $T$ 划分为 $N$ 个小片段 $\{S_1, S_2, ..., S_N\}$ 4。
- **$Answer$**：最终生成的**显式答案** 555。
- **$X$**：拼接后的完整输入序列，例如在 Stage 2 中表示为 $[Q, <think>, z_1, ..., z_N, </think>, Answer]$ 6。
### 2. 潜 Token 与隐藏状态符号
- **$L_i$**：插入在显式片段之后的**特殊占位 Token** 7。它们在 Stage 1 中负责捕捉其对应片段 $S_i$ 的信息 8。
- **$h_i$**：在经过诱导掩码（LTIM）处理后，模型在 $L_i$ 位置输出的**最后层隐状态（Last-layer Hidden State）** 9。它是一个高维向量，承载了压缩后的语义信息 。
- **$l_i$**：**词汇表对数几率（Logits）** 11。通过将隐状态 $h_i$ 投影到 LM Head（$W$）得到，公式为 $l_i = W^\top h_i$ 12。
- **$\alpha_i$**：**词汇表概率分布（Probability Vector）** 1313。对 $l_i$ 进行 Softmax 处理后得到，决定了不同词汇在潜 Token 中的“混合比例” 14141414。
- **$z_i$**：生成的**潜 Token（Latent Token / Soft Embedding）**。它是词表嵌入矩阵 $E$ 的线性组合，即 $z_i = \sum \alpha_{i,v} e_v$ 。
### 3.模型与参数相关符号
- **$\theta_{enc}$**：潜 Token 编码器（Latent Token Encoder）的参数 。它负责根据显式文本生成潜 Token 的标签 
- **$\theta_{llm}$**：大语言模型（LLM）的主体参数，在 Stage 2 中被训练为能够自主生成潜 Token 并给出答案。
- **$E$**：**词表嵌入矩阵（Embedding Matrix）**，包含词汇表中所有 Token 的向量表达 20。    
- **$W$**：**语言模型头（LM Head）**，用于将隐藏状态映射回词汇空间。
### 4. 损失函数与评价指标
- **$\mathcal{L}_{sup}$**：Stage 1 的**监督解码损失**，确保潜 Token 能正确解码出后续内容 22。
- **$\mathcal{L}_{auto}$**：Stage 2 的**自动推理损失**，由针对潜位置的 **KL 散度项**和针对显式位置的 **CE（交叉熵）项**组成 23。
- **$r$**：**压缩比（Compression Ratio）**，代表一个潜 Token 对应多少个显式 Token。
- **$ECR@K$**：**有效压缩率（Effective Compression Rate）**，用于衡量潜 Token 对显式信息的覆盖和压缩程度 。
- **$N_{eff}$**：**有效全局并行度（Effective Global Parallelism）**，用于量化潜推理过程中同时探索的路径数量 26262626。