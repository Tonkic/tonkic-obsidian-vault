---
tags:
  - 计算机
  - 人工智能
  - 工程与框架
  - Transformers实战
---
##### trainloader = DataLoader(trainset, batch_size=32, shuffle=True, collate_fn=collate_func)
其中collate_func需要自己编写，比如指明batch字典中\[0\]是text，\[1\]label