---
aliases:
  - PINN
  - Physics-Informed Neural Networks
---
### 概念
解决涉及偏微分方程（PDEs）的问题的神经网络
### 两种技术路线
![../../../pic/Pasted image 20250812022704.png](../../../pic/Pasted%20image%2020250812022704.png)
数据驱动直接用已有的 **精确解（标签数据）** u 来训练神经网络。
物理驱动是直接让网络输出的 $\hat{u}$满足 PDE 方程本身（即 PDE 残差为零）。把神经网络预测解 $\hat{u}(x,t)$ 代入 PDE 方程，看方程左右两边差多少，这个差就是残差
### PINN架构图
![Pasted image 20250815175453.png](../../../pic/Pasted%20image%2020250815175453.png)PDE loss:计算神经网络输出的各阶导数（自动微分获取），代入 PDE得到的结果u与u ̂做差后计算均方误差
BC loss:在边界x=0和x=L处采样计算u(0,t)和u(L,t)的差异；使用边界束缚网络预测
IC loss:将其中一个变量置0，神经网络预测的u ̂不能与这个值相差过大
由于BC loss与IC loss，拟合的边界值和起点终点一开始就与目标比较接近

### PINN自动微分
使用链式法则，将求导过程画成一个有向无环图，不能计算的就先放一边，所有链条都补齐了就能一步步求导出来，自动微分输入的不是函数，而是一个个有具体值的向量
反向模式可视化：
![Pasted image 20250815175814.png](../../../pic/Pasted%20image%2020250815175814.png)
### PINN局限
NIPS 2021：Characterizing possible failure modes in physics-informed neural networks提出将方程作为正则项导致了loss函数难以优化，鞍点太多，使得KINNs在面对复杂PDEs时误差大
训练难度大，收敛慢
计算成本大，对于不同的初始条件和边界条件需要重新训练