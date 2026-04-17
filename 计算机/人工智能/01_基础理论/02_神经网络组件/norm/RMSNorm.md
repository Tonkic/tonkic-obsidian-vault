---
tags:
  - 计算机
  - 人工智能
  - 基础理论
  - 神经网络
  - 归一化
  - RMSNorm
---
![Pasted image 20250924213316](../../../../../pic/Pasted%20image%2020250924213316.png)

由于norm涉及memory move，所以虽然其占用的floats计算量非常小，但将LayerNorm优化为RMSNorm依然能够缩短用时并且表现差不多