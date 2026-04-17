---
tags:
  - 计算机
  - 人工智能
  - 论文阅读
  - 文生图
  - 可控图像生成
---
![Pasted image 20251125151409](../../../../../pic/Pasted%20image%2020251125151409.png)从视频剪辑中随机选择两个帧，屏蔽一个帧的一些区域，并使用另一帧的信息学习恢复屏蔽的区域。
大量训练后，这个双U-Net就可以学会zero-shot图像编辑能力

##### attention层面的concat
$$Attention = \text{softmax}\left(\frac{Q_{i}\cdot \text{cat}(K_{i},K_{r})^{T}}{\sqrt{d_{k}}}\right) \cdot \text{cat}(V_{i},V_{r})$$
在序列长度维度拼接K和V