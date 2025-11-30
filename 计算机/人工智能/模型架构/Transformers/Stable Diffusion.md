![](../../../../pic/Pasted%20image%2020251127010635.png)重建分支（reconstruction branch）和条件分支（condition branch）
$x$ (Input Image)，$\mathcal{E}$ (Encoder 编码器/VAE)，$\mathcal{D}$ (Decoder 解码器)
Diffusion Process：不断加噪的过程，知道变成纯噪声$z_T$
$\tau_\theta$ ： Domain Specific Encoder: 在 Stable Diffusion 中通常是 CLIP 
Cross-Attention：U-Net 某一层的中间特征图和CLIP得到的文本特征做cross attention