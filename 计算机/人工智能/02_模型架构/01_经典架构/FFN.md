---
tags:
aliases:
  - FFN
  - 前馈神经网络
---
### 本质就是一个两层的[[MLP.md]]
- 两个W矩阵分别对应两层
- 原始[[Transformer.md|transformer]]中的FFN:
![[../../../../pic/Pasted image 20250924213755.png]]

- 现代大多数transformer中的FFN:(原因见[[../../工程与框架/Transformers实战/dropping bias.md|dropping bias]])
![[../../../../pic/Pasted image 20250808182745.png]]