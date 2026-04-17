---
tags:
  - 计算机
  - 人工智能
  - 基础理论
  - 训练与优化
  - 指标
  - FID
---
---
aliases:
  - Fréchet Inception Distance
---
### Intuitive understanding
直观感受，FID是反应生成图片和真实图片的距离，数据越小越好
### 实现
FID使用Inception Net-V3全连接前的2048维向量作为图片的feature。
![Pasted image 20251106132051](../../../../../pic/Pasted%20image%2020251106132051.png)
[协方差](../../../../../数学/概率论/协方差.md.md)
