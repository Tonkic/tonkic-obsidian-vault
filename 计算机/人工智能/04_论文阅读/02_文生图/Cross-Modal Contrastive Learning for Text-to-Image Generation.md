将对比学习（使用正负样本）用在文生图上

![[../../../../pic/Pasted image 20260307222428.png]]
图中有三类主体：**Real Image**（真实的参考图）、**Caption**（文本描述），以及由 Generator（生成器）生成的**假图**

##### Generator 和 Critic都是CNN
**Generator（生成器）**：它的主体是由多个**上采样残差块 (Up-sampling Residual Blocks)** 组成的 。输入特征生成图片
**Critic（判别器/评判器）**：它的主体是由多个**下采样残差块 (Down-sampling Residual Blocks)** 组成的，输入图片压缩为特征

为了防止 Critic 被 Generator 糟糕的早期作品带偏，作者在架构上做了一个强硬的规定：
- **Critic 里的对比学习投影层（负责画图1红绿线特征的那个大脑），只允许使用“真实的图片和真实的描述”来进行训练！**