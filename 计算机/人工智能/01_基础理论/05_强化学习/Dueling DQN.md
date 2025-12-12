[DQN](DQN.md) 改进算法

根据定义，状态价值 $V(s)$ 是动作价值 $Q(s, a)$ 在当前策略 $\pi$ 下的期望（Expectation）：

$$V(s) = \mathbb{E}_{a \sim \pi}[Q(s, a)]$$

Dueling DQN 的网络结构将 $Q$ 值拆分为两路输出：$V$ 和 $A$。

$$Q(s, a) = V(s) + A(s, a)$$

```python
def forward(self, x):
        A = self.fc_A(F.relu(self.fc1(x)))
        V = self.fc_V(F.relu(self.fc1(x)))
        # Q值计算：核心公式
        Q = V + A - A.mean(1).view(-1, 1)  
        return Q
```
A - A.mean(1)使得处理后的优势值之和（以及均值）恒为 0。使得V(s)被迫去拟合Q的均值