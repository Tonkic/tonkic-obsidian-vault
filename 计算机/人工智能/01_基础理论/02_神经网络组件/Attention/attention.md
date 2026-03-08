---
tags:
  - 计算机
  - 人工智能
  - 基础理论
  - 神经网络
  - 注意力机制
---
本质是求相似度的一个算子

### 输入输出
输入：token/tensor
输出：token/tensor
但是输出的token里面的值理解了上下文的含义

### 重复attention
大模型中往往不止一层attention，而是会重复叠多层attention，信息“注意”到信息的范围会随着attention的层数增加而增加，如：
- 第 1 层的 Attention：可能只关注到了旁边的词（语法关系）。
- 第 15 层的 Attention：可能关注到了几千字之前的伏笔（长距离依赖）。
- 第 32 层的 Attention：已经形成了非常抽象和深层的语义理解。