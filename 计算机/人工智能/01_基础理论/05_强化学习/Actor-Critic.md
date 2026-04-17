---
tags:
  - 计算机
  - 人工智能
  - 基础理论
  - 强化学习
---
用 Critic 网络估计的价值 替代了 REINFORCE 中的 完整轨迹回报 $G_t$
改进了[REINFORCE](REINFORCE.md.md)方法需要完整走完一整个轨迹的缺点，而是使用Critic网络的预测值来给出对未来的预期 $V(s_{t+1})$ ，避免了某一次随机实验偏差过大的情况