---
tags:
  - NeurIPS2025
  - 开源论文
---
### Long-term Consistent World Simulation with Memory
[https://github.com/xizaoqu/WorldMem]

### 核心工作
提出 WORLDMEM，一个面向长时世界模拟（world simulation）的记忆增强框架。核心目标是解决传统视频世界模型由于上下文窗口有限，导致“走远再回来时场景不一致”的问题。

作者的关键思想是：
- 不显式重建完整 3D 场景；
- 而是维护一个 memory bank，存储历史帧的 latent token 以及对应状态；
- 在当前生成时，从 memory bank 中检索最相关的历史记忆；
- 再通过 state-aware memory attention 将这些记忆注入生成过程，从而实现长期一致的世界生成。

该方法不仅能保持 3D 空间一致性，还能通过时间戳建模世界的动态变化，使模型具备一定的“时间演化记忆”能力。

### 方法
##### 1. 记忆表示与记忆库构建 (Memory Representation and Memory Bank)
为了突破生成模型固定上下文窗口的限制，作者为模型引入了外部记忆库 memory bank。

- **记忆单元组成：** 每个 memory unit 由两部分构成：
  - 历史帧的 latent token 表示；
  - 与该帧对应的状态 state，包括：
    - 相机位姿 pose；
    - 时间戳 timestamp。
- **形式化表示：** 记忆单元可写作：
  - `(x_i^m, p_i, t_i)`
  - 其中 `x_i^m` 是历史记忆帧，`p_i` 是位姿，`t_i` 是时间。
- **设计动机：** 作者认为，与其做刚性的显式 3D 重建，不如直接保留历史视觉 token。这样既能保留细节，又更适合动态环境中的长期生成与交互。

##### 2. 记忆检索机制 (Memory Retrieval)
由于 memory bank 会越来越大，模型不可能每次都读取全部历史记忆，因此需要一个高效的检索机制。

- **检索目标：** 从大量历史记忆中选出与当前生成状态最相关的若干 memory units。
- **打分依据：**
  - 当前视角与历史视角之间的 FOV overlap（视场重叠）；
  - 当前时间与历史时间之间的 temporal distance（时间差）。
- **基本思想：**
  - 视场越重叠，说明这个历史帧越可能与当前场景相关；
  - 时间越接近，通常也越值得优先参考。
- **检索策略：**
  - 先根据置信度分数进行排序；
  - 再使用 similarity filtering 去除过于相似、冗余的记忆；
  - 最终保留固定数量的高质量记忆帧作为后续条件输入。
- **优点：** 这个策略实现简单，但能有效筛出与当前场景真正相关的历史内容。

##### 3. 状态感知记忆注意力 (State-aware Memory Attention)
拿到检索出来的 memory units 后，作者通过 cross-attention 将记忆融入当前生成过程。

- **基本做法：**
  - 当前正在生成的 latent feature 作为 query；
  - 检索得到的 memory feature 作为 key 和 value；
  - 通过 cross-attention 从历史记忆中提取有用信息。
- **问题：** 如果只用视觉 token，模型很难判断“这是不是同一个地方、只是视角变了”。
- **解决方式：** 作者将 state embedding 一起加入 query 和 key：
  - query 特征加上当前状态 embedding；
  - key 特征加上历史状态 embedding。
- **效果：** 这样 attention 不只是“看图像像不像”，而是在做带位姿与时间信息的时空对齐与推理。

##### 4. 状态嵌入设计 (State Embedding)
为了让模型能够利用位姿和时间信息，作者设计了专门的 state embedding。

- **空间状态编码：**
  - 使用 Plücker embedding 对 5D pose（位置 + 朝向）进行编码；
  - 再通过 MLP 投影到统一特征空间。
- **时间状态编码：**
  - 对 timestamp 做 sinusoidal embedding；
  - 再经过轻量 MLP 得到时间特征。
- **最终状态表示：**
  - pose embedding 与 time embedding 相加，形成统一的 state embedding。
- **意义：**
  - pose 告诉模型“是从哪里看”；
  - time 告诉模型“是在什么时候看”。

##### 5. 相对状态建模 (Relative State Formulation)
作者发现，直接使用绝对 pose 和绝对时间不利于学习长期对应关系，因此进一步采用 relative state 设计。

- **具体做法：**
  - 当前 query 帧的状态被设为零参考；
  - memory key 的状态改为相对当前帧的位姿偏移和时间偏移。
- **优势：**
  - 模型不必学习复杂的全局绝对坐标；
  - 只需要学习“历史场景相对于当前场景的变化关系”；
  - 更利于跨视角、跨时间的稳定对齐。

##### 6. 与基础世界模型的结合方式 (Integration into Diffusion Transformer)
WORLDMEM 建立在 Conditional Diffusion Transformer + Diffusion Forcing 的自回归视频生成框架之上。

- **基础模型：**
  - 使用 conditional DiT 进行视频生成；
  - 使用 Diffusion Forcing 支持长视频自回归生成。
- **记忆融入方式：**
  - memory frames 在训练和推理时都作为“干净条件帧”输入；
  - 同时通过专门的 memory block，把历史记忆注入当前生成过程。
- **额外设计：**
  - 作者还使用 temporal attention mask，限制 memory 只影响当前生成，不让不同 memory unit 之间互相干扰。

### 实验结论
##### 1. Minecraft 长期一致性实验
作者在 Minecraft 中评估了模型在可交互环境中的世界一致性。

- **within context window：**
  - 测试短期回头看时的自洽性；
  - WORLDMEM 优于 full-sequence 和 Diffusion Forcing。
- **beyond context window：**
  - 给定长达 600 帧的历史记忆，再生成未来 100 帧；
  - 普通方法在超出上下文后明显失去一致性；
  - WORLDMEM 仍能较好恢复过去看过的场景。
- **说明：**
  - 该方法确实学会了“记住世界”，而不是仅靠短期上下文猜测。

##### 2. RealEstate10K 真实场景实验
作者还在真实室内视频数据上验证空间一致性。

- **测试方式：**
  - 设计闭环相机轨迹，让相机转一圈再回到原点；
  - 对比第一帧和最后一帧是否一致。
- **结果：**
  - WORLDMEM 在 PSNR、LPIPS、rFID 上优于 CameraCtrl、TrajAttn、ViewCrafter 和 DFoT；
  - 表明该方法不只适用于虚拟环境，也能在真实场景中提升 3D 空间一致性。

##### 3. 消融实验
作者做了多组消融来验证各模块的重要性。

- **状态嵌入设计：**
  - Dense pose embedding 优于 sparse embedding；
  - Relative embedding 优于 absolute embedding。
- **记忆检索策略：**
  - Random retrieval 效果最差；
  - 加入 confidence filtering 和 similarity filtering 后显著提升性能。
- **时间条件：**
  - 去掉 timestamp 后，模型无法区分“同一地点在不同时刻的状态”；
  - 加入时间条件后，动态事件建模更准确。
- **训练采样策略：**
  - Progressive sampling 优于固定小范围或大范围采样；
  - 说明逐步增加检索难度有助于模型学会稳定地使用 memory。

### 方法优点
- 直接针对长时世界一致性问题，目标明确；
- 不依赖显式 3D 重建，方法更灵活；
- 记忆表示保留视觉细节，适合动态环境；
- 引入位姿与时间作为状态条件，提升跨视角、跨时间推理能力；
- 在虚拟场景和真实场景中都有效。

### 局限性
- memory retrieval 仍不能保证总能取到最关键的信息，尤其在遮挡严重时；
- 当前交互类型还不够丰富，真实世界复杂动态场景仍需进一步验证；
- memory bank 会随时间线性增长，超长序列下存储与计算成本仍然较高。

### 总结
WORLDMEM 的本质，是给 world model 加上一个“可检索、可对齐、带状态的长期记忆系统”。

它不是简单把过去帧拼回去，而是：
- 存储历史视觉记忆；
- 用位姿和时间辅助检索；
- 再用 state-aware attention 做时空推理。

因此，模型不仅能“记住看过什么”，还能更好地理解“是从哪里看见的、在什么时候看见的”，从而实现更长期、更一致的世界生成。