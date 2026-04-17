---
tags:
  - 计算机
  - 人工智能
  - 工程与框架
  - PyTorch
---
---
aliases:
  - 张量
---
tensor(张量)本质是指向一片连续内存地址的一个指针
- 所以很多改变view的操作并不会进行copy
- 逐元素操作/转置等操作通常会导致不连续

### 维度变换
`torch.view()`更改其中的一个，另外一个也会跟着改变
```python
# 首先需要保证tensor是contiguous的，才能使用view，view后的顺序也是按照内存中的顺序
a=torch.Tensor([[1,2,3]([1,2,3)])
print(a.view(3,2))
""" >>> tensor([[1., 2.],
        [3., 4.],
        [5., 6.]]) """
a.view(3,-1) #有时候会出现-1的情况表示自适应
a.contiguous() #如果不是contiguous的，使用这个方法变为contiguous，会copy一份
```
`torch.reshape()`创建新张量并改变张量的形状，官方不推荐使用。推荐的方法是我们先用 `clone()` 创造一个张量副本然后再使用 `torch.view()`进行函数维度变换

### 广播机制

当对两个形状不同的 Tensor 按元素运算时，可能会触发广播(broadcasting)机制：先适当复制元素使这两个 Tensor 形状相同后再按元素运算。
```python
x = torch.arange(1, 3).view(1, 2)
print(x)
y = torch.arange(1, 4).view(3, 1)
print(y)
print(x + y)

tensor([1, 2](1,%202))
tensor([[1],
        [2],
        [3]])
tensor([[2, 3],
        [3, 4],
        [4, 5]])
```

由于x和y分别是1行2列和3行1列的矩阵，如果要计算x+y，那么x中第一行的2个元素被广播 (复制)到了第二行和第三行，⽽y中第⼀列的3个元素被广播(复制)到了第二列。如此，就可以对2个3行2列的矩阵按元素相加。  
