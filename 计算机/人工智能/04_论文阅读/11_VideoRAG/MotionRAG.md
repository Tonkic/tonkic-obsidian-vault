---
tags:
  - NeurIPS2025
---
### Motion Retrieval-Augmented Image-to-Video Generation
![[../../../../pic/Pasted image 20260415011248.png]]

### 核心工作
将RAG思想用于 Image-to-Video 任务。论文提出 MotionRAG：先根据文本从视频库中检索出具有相似运动模式的参考视频，再把这些视频中的高层运动信息迁移到目标图像上，最后注入到现有 I2V 扩散模型中，提升生成视频的运动真实性与物理合理性。
### 方法
##### 1. 基于文本的视频检索 (Text-based Video Retrieval)
模型首先不直接“凭空想象”运动，而是先去数据库里找“运动参考例子”。
- 建库：作者为每个视频准备对应 caption，并用文本编码模型把 caption 编码成向量，作为检索索引，存入向量库中。论文实现里用的是 GTE 系列文本嵌入模型，因此它本质上是“文本检索视频”，不是直接做视频特征检索。
- 检索：推理时，输入 prompt 先编码成文本向量，再和数据库中所有视频 caption 向量做余弦相似度，取 Top-K 个最相关视频作为参考。作者默认用 top-9。
##### 2. 上下文感知的运动适配 (CAMA: Context-Aware Motion Adaptation)
分三步：
- 运动特征提取：
  作者用预训练 VideoMAE 编码参考视频，提取表示运动的特征。与光流这类低层像素运动不同，这些特征更偏向“语义层面的动作模式”。随后再通过一个可学习的 resampler，把原本密集的时空特征压缩成固定数量的 motion tokens，作为每个参考视频的运动表示。
- 外观特征提取：
  为了知道“这些运动该迁移到谁身上”，作者还用 DINOv2 提取目标图像以及每个参考视频首帧的 appearance features。同样通过 resampler 压缩成与 motion tokens 数量、维度一致的 image tokens。这样，模型就同时拿到了“这个视频是怎么动的”和“这个主体长什么样”。
- 运动适配 / in-context learning：
  这是整篇论文最核心的设计。作者没有直接把检索到的运动特征平均一下，而是构造了一个因果 Transformer，把多个“参考视频首帧外观 + 对应运动特征”按顺序喂进去，让模型像 in-context learning 一样，从这些例子里学会“某种外观通常该匹配什么样的运动”，最后再为目标图像预测一个适配后的目标运动表示。  
  一个很有意思的细节是：作者把检索结果按“从低相关到高相关”的逆序排列，让模型先看不太相关的例子，再逐渐过渡到更相关的例子，最后处理目标图像。这样相当于让模型逐步建立 motion-appearance 对应关系，再对目标做推断。
##### 3. 运动引导的视频生成 (Motion-Guided Video Generation)
有了适配后的目标运动特征，接下来就是把它真正注入到视频扩散模型中。
- 论文把生成阶段建立在现有预训练 I2V 模型上，比如 SVD、DynamiCrafter、CogVideoX。原来的条件通常只有输入图像和文本，现在扩展为“图像 + 文本 + 运动特征”。
- 为了不破坏原模型，作者采用类似 IP-Adapter 的思路，设计了一个 Motion-Adapter。具体做法是在 UNet 或 DiT 主干中的注意力层后，额外插入基于 motion features 的 cross-attention，让当前隐特征去关注预测出的运动 tokens。这样生成器在时间建模时，就能被外部真实视频中的运动先验所引导。
- 训练时，基础生成模型参数冻结，只训练 Motion-Adapter 和相关适配模块，因此它是对现有 I2V 模型的轻量增强，而不是重新训练整套视频生成系统。
##### 4. 两阶段训练策略
为了让整个系统稳定工作，作者采用两阶段训练。
- 第一阶段：训练 Motion-Adapter 和 motion resampler，让生成模型先学会“如果给我正确的 motion features，我就能把它们转成视频动态”。这一阶段直接使用真实视频提取到的运动特征作为条件。
- 第二阶段：冻结 motion resampler，训练 Motion Context Transformer 和 image resampler。目标是让 Transformer 学会根据一组检索上下文，预测出目标图像应对应的 motion representation，并用 L2 loss 去拟合真实目标视频的 motion features。
### 方法直觉
可以把 MotionRAG 理解成：
- 普通 I2V：给一张图和一句话，让模型自己脑补怎么动。
- MotionRAG：先去真实世界视频库里找“类似动作范例”，再提炼这些范例中的高层运动规律，适配到当前主体外观上，最后把这种运动先验喂给扩散模型。
所以它解决的问题不是“怎么生成更清晰的视频”，而是更聚焦于：
- 动作是否更自然
- 物理运动是否更合理
- 主体是否不再静止或乱动
- 能不能跨外观迁移运动模式
这也是论文里“astronaut riding a horse on the moon”这个例子的核心：虽然宇航员和骑马视频长得不一样，但“骑马的运动模式”是可迁移的。

MotionRAG 的本质不是“给视频生成模型加知识库”，而是“给视频生成模型加一个可检索、可迁移的运动先验库”。  
它的核心贡献在于把“运动迁移”做成了一个 in-context learning 问题：从多个参考视频中学习 motion-appearance 对应关系，再把合适的运动注入目标图像生成过程，从而让 I2V 生成的视频动得更自然、更像真的。