### 传统的 Classifier guidance
需要一个diffsion模型+一个Classifier分类器,最终输出的logits=
∇xlogpγ(x∣y)=∇xlogp(x)+γ∇xlogp(y∣x).

x是高位输入,y是label,右边第一项就是不做引导自然生成,第二项是对于输入x,分类器预测其label的概率

### Classifier-free guidance
不需要Classifier分类器,而是一个