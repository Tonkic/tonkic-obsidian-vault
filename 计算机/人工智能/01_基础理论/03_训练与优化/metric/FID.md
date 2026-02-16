---
aliases:
  - Fréchet Inception Distance
---
### Intuitive understanding
直观感受，FID是反应生成图片和真实图片的距离，数据越小越好
### 实现
FID使用Inception Net-V3全连接前的2048维向量作为图片的feature。
![[../../../../../pic/Pasted image 20251106132051.png]]
[[../../../../../数学/概率论/协方差.md]]
