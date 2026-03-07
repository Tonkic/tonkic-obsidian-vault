将对比学习（使用正负样本）用在文生图上

![[../../../../pic/Pasted image 20260307222428.png]]
图中有三类主体：**Real Image**（真实的参考图）、**Caption**（文本描述），以及由 Generator（生成器）生成的**假图**

##### Generator 和 Critic都是CNN
**Generator（生成器）**：它的主体是由多个**上采样残差块 (Up-sampling Residual Blocks)** 组成的 。输入特征生成图片
**Critic（判别器/评判器）**：它的主体是由多个**下采样残差块 (Down-sampling Residual Blocks)** 组成的，输入图片压缩为特征