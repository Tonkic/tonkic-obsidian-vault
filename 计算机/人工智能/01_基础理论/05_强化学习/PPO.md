---
tags:
  - 计算机
  - 人工智能
  - 基础理论
  - 强化学习
---
---
aliases:
  - Proximal Policy Optimization
---
$r_t(\theta)$衡量 更新后的策略在当前状态下选择这个动作的概率，是旧策略的多少倍。是概率值的比

$$r_t(\theta) = \frac{\pi_{new}(a_t|s_t)}{\pi_{old}(a_t|s_t)}$$

##### PPO的改进
当$r_t(\theta)$大于/小于某个阈值时，将loss导数变为0，不再反向传播更新神经网络参数