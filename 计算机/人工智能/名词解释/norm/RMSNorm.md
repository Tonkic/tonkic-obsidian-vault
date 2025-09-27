![](../../../../pic/Pasted%20image%2020250924213316.png)

由于norm涉及memory move，所以虽然其占用的floats计算量非常小，但将LayerNorm优化为RMSNorm依然能够缩短用时并且表现差不多