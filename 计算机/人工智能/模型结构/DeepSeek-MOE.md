### 概念
Experts：专家网络，是[FFN](../深度学习模型/前馈神经网络.md)
Router：路由/门控网络，控制选择哪个专家的“开关”，由FFNN+Softmax组成
![../../../pic/Pasted image 20250808184305.png](../../../pic/Pasted%20image%2020250808184305.png)

### 分类
Dense MoE：每次路由的时候，所有的专家都有可能被选中
Sparse MoE：路由的时候，使用topK的专家，最后将几个专家聚合aggregated.

### 负载均衡
解决训练的时候会自动选择计算速度较快的专家的问题
#### 方法
keep topK：当某个专家选择过多的时候，加入高斯噪声降低他的得分
Auxiliary Loss/Balancing Loss：
![Pasted image 20250808200349.png](../../../pic/Pasted%20image%2020250808200349.png)
- $\alpha_1$：超参数，称为专家级平衡因子（expert-level balance factor），用于调整平衡损失在总损失中的权重。
- $N'$：路由专家的总数，$N'$ = $mN - K_s$​（其中m是每个FFN专家被划分为m个更小的专家、N是专家的总数、$K_s$是共享专家的数量）。
- $f_i$：专家i的频率（frequency），表示专家i被选择的频率。
- $P_i$​：专家i的平均重要性（average importance），表示专家i在所有时间步上的平均重要性得分。
- $s_{i,l}$表示token $u_i$与第t个专家的**亲和度分数**（Affinity Score），使用softmax计算
Expert Capacity：专家容量，每个专家最多可以处理几个tokens，设置为3
#### 代码实现
```python
if self.training:
            # 重要性损失（专家利用率均衡）
            importance = probs.sum(0) # 当前专家对每一个tokens求和，表示专家的重要性
            importance_loss = torch.var(importance) / (self.num_experts ** 2)
            
            # 负载均衡损失（样本分配均衡）
            mask = torch.zeros_like(probs, dtype=torch.bool)
            mask.scatter_(1, topk_indices, True) # 第一个设置为共享专家，然后在其余的专家里面选择TopK
            routing_probs = probs * mask 
            expert_usage = mask.float().mean(0) # 曾经的专家选择频率
            routing_weights = routing_probs.mean(0) # 这次选择了哪些专家
            load_balance_loss = self.num_experts * (expert_usage * routing_weights).sum() # 希望以前选的多的专家现在选得少一点
            
            aux_loss = importance_loss + load_balance_loss
        else:
            aux_loss = 0.0
```

