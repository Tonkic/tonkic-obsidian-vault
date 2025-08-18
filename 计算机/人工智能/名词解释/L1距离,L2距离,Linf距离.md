---
tags:
  - 机器学习
aliases:
  - MAE
  - MSE
  - L1距离
  - L2距离
  - Linf距离
---
![../../../pic/Pasted image 20250223222224.png](../../../pic/Pasted%20image%2020250223222224.png)


## 均方误差(MSE)和均方根误差(RMSE)和平均绝对误差(MAE)
MAE(mean absolute error)，即平均绝对值误差，也可以看做L1损失，是一种用于回归模型的常用损失函数。MAE是目标值和预测值之差的绝对值之和

MSE(mean squared error)，即均方误差，可以看做是一种L2损失，也是一种最常用的回归损失函数。MSE是求预测值与真实值之间距离的平方和。

简单来说，MSE计算简便，但MAE对异常点有更好的鲁棒性。下面就来介绍导致二者差异的原因。