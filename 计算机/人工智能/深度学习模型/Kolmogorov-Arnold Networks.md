---
aliases:
  - KAN
---
### 与传统机器学习MLP的主要区别
- 不训练权重，而是训练激活函数
- MLP如果需要无限逼近，理论上需要一个无穷宽度的网络
- KAN可以使用有限大小的网络实现无穷精度，但是必须确保激活函数是光滑的

### 核心定理
![../../../pic/Pasted image 20250815173833.png](../../../pic/Pasted%20image%2020250815173833.png)
意思是一个**有限**的多元函数，可以拆分成多个**一元函数**和**加法**，奇妙的是，只需要加法即可，不需要乘除开方等复杂操作
一元函数的优势： 可以使用曲线描绘，可视化比较清楚，同时可以通过多个函数求和拟合

### network架构
![Pasted image 20250815175217.png](../../../pic/Pasted%20image%2020250815175217.png)
- 每一个激活函数可以被多个**光滑**的函数得到
- 如果固定两层会比较病态pathological，如果是三层可以学习到各种特征，比如exp函数，sin/cos函数，幂函数三种分三层
- 可以将不同的$B_(x)$固定，真正学习的只是前面的参数$C_i$

### B-spline
B-spline表示为$B_{i,k}$，其中k表示k阶贝塞尔函数

$$
B_{i,k} = \begin{cases} 
\begin{cases} 
1, & t_i < t < t_{i+1} \\
0, & \text{else}
\end{cases}, & k=1 \\
\frac{t - t_i}{t_{i+k-1} - t_i} B_{i,k-1}(t) + \frac{t_{i+k} - t}{t_{i+k} - t_{i+1}} B_{i+1,k-1}(t), & k \geq 2
\end{cases}
$$

递归的，$B_{i,k}$需要$B_{i,k-1}$和$B_{i-1,k-1}$
### Residual activation functions残差激活函数
加入b(x)作为残差
$\phi(x) = w_b b(x) + w_s \,\text{spline}(x)$

$b(x) = \text{silu}(x) = \frac{x}{1 + e^{-x}}$
- spline初始化为几乎是0
- 实现除法：先log，用减法，再exp回来
- silu是[sigmoid](../名词解释/sigmoid.md)函数的一个变体
#### 贝塞尔函数
![Pasted image 20250818225045.png](../../../pic/Pasted%20image%2020250818225045.png)
