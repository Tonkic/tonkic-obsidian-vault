### AE结构
Encoder压缩到latent space 然后Decoder解码
### VAE相对AE的改进
不使用一个具体的向量来表示encoder后的embedding，而是使用一个概率分布p(z|x)来表示现实世界的分布p(x)，z就是这个向量可能的范围，我们近似的将其认为是一个高斯分布
### 具体实现
Encoder会把x映射为高斯分布的参数$\mu$ 和 $\sigma$
##### Loss
![](../../../pic/Gemini_Generated_Image_10y85k10y85k10y8.png)
前面可以看成一个L2 loss，后面是KL散度