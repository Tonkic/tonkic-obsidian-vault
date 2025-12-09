---
aliases:
  - 交叉熵损失
---
交叉熵（Cross Entropy）用来表达两个概率分布之间的相似性，熵越大则表示差别越大，损失值也就越大。
相比于[NLL Loss](NLL%20Loss.md)而言，就是只是将输入进行了Softmax之后再log
![](../../../../../pic/Pasted%20image%2020251209155435.png)