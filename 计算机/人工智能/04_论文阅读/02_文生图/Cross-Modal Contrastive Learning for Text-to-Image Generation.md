---
tags:
  - 计算机
  - 人工智能
  - 论文阅读
  - 文生图
---
将对比学习（使用正负样本）用在文生图上

![Pasted image 20260307222428](../../../../pic/Pasted%20image%2020260307222428.png)
图中有三类主体：**Real Image**（真实的参考图）、**Caption**（文本描述），以及由 Generator（生成器）生成的**假图**

### Generator 和 Critic都是CNN
**Generator（生成器）**：它的主体是由多个**上采样残差块 (Up-sampling Residual Blocks)** 组成的 。输入特征生成图片
**Critic（判别器/评判器）**：它的主体是由多个**下采样残差块 (Down-sampling Residual Blocks)** 组成的，输入图片压缩为特征
##### Critic 的“一个身子，两个脑袋”
整个 Critic（CNN 网络）由以下部分组成：
- **身子（Backbone / 前面层）：** 负责把图像层层压缩，提取基础特征。
- **脑袋 A（对抗头 / Adversarial Head）：** 负责输出 0 或 1，判断这张图是真还是假 。
- **脑袋 B（对比学习投影头 / Projection Head）：** 负责输出特征向量，用于和文本算相似度（画红绿线） 。

为了防止 Critic 被 Generator 糟糕的早期作品带偏，作者在架构上做了一个强硬的规定：
- **Critic 里的对比学习投影层（负责画图1红绿线特征的那个大脑），只允许使用“真实的图片和真实的描述”来进行训练！**（脑袋B无法进行梯度回传）