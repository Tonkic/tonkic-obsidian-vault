$d_{\text{head}}\times num_{\text{heads}} = d_{\text{model}}$ 
- 表示等号两边一般是1：1，但不是强制的
- 把向量切成 h 个小头并行算，每个头的维度是 d_model/h，h 个头的总维度仍是 d_model
