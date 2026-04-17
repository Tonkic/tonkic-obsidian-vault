---
tags:
aliases:
  - FFN
  - 前馈神经网络
---
### 本质就是一个两层的[MLP](MLP.md.md)
- 两个W矩阵分别对应两层
- 原始[transformer](Transformer.md.md)中的FFN:
![Pasted image 20250924213755](../../../../pic/Pasted%20image%2020250924213755.png)

- 现代大多数transformer中的FFN:(原因见[dropping bias](../../工程与框架/Transformers实战/dropping%20bias.md))
![Pasted image 20250808182745](../../../../pic/Pasted%20image%2020250808182745.png)