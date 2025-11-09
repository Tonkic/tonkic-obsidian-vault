---
aliases:
  - Kullback-Leibler Divergence
---
### 直观解释
抽象：两种概率分布之间的距离
**KL散度描述了我们用分布Q来估计数据的真实分布P的编码损失**。
### 公式
对于离散随机变量，其概率分布$P$和$Q$的KL散度可按下式定义为

$$D_{KL}(P \mid\mid Q) = -\sum_{i} P(i) \ln \frac{Q(i)}{P(i)}$$

等价于

$$D_{KL}(P \mid\mid Q) = \sum_{i} P(i) \ln \frac{P(i)}{Q(i)}$$

即按概率$P$求得的$P$和$Q$的对数商的平均值。KL 散度仅当概率$P$和$Q$各自总和均为1，且对于任何$i$皆满足$Q(i) > 0$及$P(i) > 0$时，才有定义。式中出现$0 \ln 0$的情况，其值按0处理。

对于连续随机变量，其概率分布$P$和$Q$的KL散度可按积分方式定义为

$$D_{KL}(P \mid\mid Q) = \int_{-\infty}^{\infty} p(x) \ln \frac{p(x)}{q(x)} dx$$

其中$p$和$q$分别表示分布$P$和$Q$的密度。