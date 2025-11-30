---
tags:
aliases:
  - FFN
  - 前馈神经网络
---
### 本质就是一个两层的[MLP](经典骨干/MLP.md)
- 两个W矩阵分别对应两层
- 原始[transformer](经典骨干/Transformer.md)中的FFN:
![](../../../../pic/Pasted%20image%2020250924213755.png)

- 现代大多数transformer中的FFN:(原因见[dropping bias](../../工程与框架/Transformers实战/dropping%20bias.md))
![../../../../pic/Pasted image 20250808182745.png](../../../../pic/Pasted%20image%2020250808182745.png)