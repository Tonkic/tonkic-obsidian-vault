相比平方误差损失，Huber损失对于数据中异常值的敏感性要差一些。在值为0时，它也是可微分的。它基本上是绝对值，在误差很小时会变为平方值。误差使其平方值的大小如何取决于一个超参数δ，该参数可以调整。**当δ~ 0时，Huber损失会趋向于MAE；当δ~ ∞（很大的数字），Huber损失会趋向于MSE。**
![[../../../pic/Pasted image 20250813162609.png]]
```python
# huber 损失
def huber(true, pred, delta):
    loss = np.where(np.abs(true-pred) < delta , 0.5*((true-pred)**2), delta*np.abs(true - pred) - 0.5*(delta**2))
    return np.sum(loss)

```