$d_{\text{head}}\times num_{\text{heads}} = d_{\text{model}}$
把向量切成 h 个小头并行算，每个头的维度是 d_model/h，h 个头的总维度仍是 d_model

- 一般head dimension和the number of heads的比例是1：1