## Classifier Guidance 和 Classifier-Free Guidance 的区别

### 1. 传统 Classifier Guidance
传统的 **Classifier Guidance** 需要两个模型：
1. 一个无条件 diffusion model；
2. 一个额外训练的 classifier 分类器。
无条件 diffusion model 负责建模图像分布 $p(x)$，也就是“如何生成自然图像”。
classifier 负责建模条件概率 $p(y\mid x)$，也就是“给定图像 $x$，它属于类别 $y$ 的概率是多少”。
根据贝叶斯公式：

$$  
p(x\mid y)=\frac{p(y\mid x)p(x)}{p(y)}  
$$

两边取 $\log$：

$$  
\log p(x\mid y)=\log p(x)+\log p(y\mid x)-\log p(y)  
$$

由于 $p(y)$ 和 $x$ 无关，所以对 $x$ 求梯度时：

$$  
\nabla_x \log p(y)=0  
$$

因此得到：
$$  
\nabla_x \log p(x\mid y)

\nabla_x \log p(x)  
+  
\nabla_x \log p(y\mid x)  
$$

在实际 guidance 中，会把条件项乘上一个 guidance scale $\gamma$：

$$  
\nabla_x \log p_\gamma(x\mid y)

\nabla_x \log p(x)  
+  
\gamma \nabla_x \log p(y\mid x)  
$$

这里需要注意：这个式子不是 logits，而是 **guided score**，也就是指导采样时 $x$ 应该往哪个方向移动的梯度。

其中：

- $\nabla_x \log p(x)$：无条件 diffusion model 提供的方向，让样本变得更像真实图像；
    
- $\nabla_x \log p(y\mid x)$：classifier 提供的方向，让样本更像目标类别 $y$；
    
- $\gamma$：guidance scale，用来放大条件引导强度。
    

更准确地说，在 diffusion 采样过程中，$x$ 通常是某个时间步的带噪样本 $x_t$，所以公式也可以写成：

$$  
\nabla_{x_t} \log p_\gamma(x_t\mid y)

\nabla_{x_t} \log p(x_t)  
+  
\gamma \nabla_{x_t} \log p(y\mid x_t)  
$$

直观理解：

$$  
\text{生成符合条件的图像}

\text{生成自然图像}  
+  
\gamma \times \text{让图像更像类别 } y  
$$

例如想生成一张狗的图像：

$$  
\text{生成狗图}

\text{生成自然图像}  
+  
\gamma \times \text{让分类器更确信这是狗}  
$$

当 $\gamma$ 越大时，生成结果越倾向于符合类别 $y$，但如果 $\gamma$ 太大，也可能导致图像失真或不自然。

---

### 2. Classifier-Free Guidance

**Classifier-Free Guidance，CFG** 不需要额外训练一个 classifier。

它的做法是：训练一个 conditional diffusion model，让同一个 diffusion model 同时学会两种模式：

1. 有条件生成：输入条件 $y$，学习 $p(x\mid y)$；
    
2. 无条件生成：不输入条件，学习 $p(x)$。
    

训练时，模型大多数时候会看到正常条件 $y$，例如类别标签或文本 prompt；但在一部分训练步骤中，比如 10% 到 20%，条件 $y$ 会被随机移除，替换成空条件 $\emptyset$。

因此，同一个模型既能在有条件时运行：

$$  
s_\text{cond}(x_t,t,y) \approx \nabla_{x_t}\log p(x_t\mid y)  
$$

也能在无条件时运行：

$$  
s_\text{uncond}(x_t,t) \approx \nabla_{x_t}\log p(x_t)  
$$

其中 $s$ 表示 score，也就是对数概率对 $x_t$ 的梯度方向。

CFG 的核心公式是：
$$  
s_\text{guided}

s_\text{uncond}  
+  
\gamma\left(s_\text{cond}-s_\text{uncond}\right)  
$$

展开后得到：
$$  
s_\text{guided}

(1-\gamma)s_\text{uncond}  
+  
\gamma s_\text{cond}  
$$

如果写成概率梯度形式，就是：
$$  
\nabla_x \log p_\gamma(x\mid y)

(1-\gamma)\nabla_x \log p(x)  
+  
\gamma \nabla_x \log p(x\mid y)  
$$

其中：

- 当 $\gamma=0$ 时，只使用无条件模型，相当于从 $p(x)$ 采样；
    
- 当 $\gamma=1$ 时，只使用标准条件模型，相当于从 $p(x\mid y)$ 采样；
    
- 当 $\gamma>1$ 时，模型会把条件方向进一步放大，这就是 CFG 的主要效果来源。
    

CFG 中最关键的部分是：

$$  
s_\text{cond}-s_\text{uncond}  
$$

这个差值可以理解为“条件 $y$ 带来的额外方向”。

也就是说：

$$  
\text{条件引导方向}
=
\text{有条件预测}
-
\text{无条件预测}  
$$

所以 CFG 可以写成：

$$  
s_\text{guided}

\text{自然图像方向}  
+  
\gamma \times \text{条件引导方向}  
$$

这就是为什么它叫 **Classifier-Free Guidance**：它不需要额外的 classifier，而是用同一个 diffusion model 的有条件预测和无条件预测之间的差值，来构造类似 classifier guidance 的条件引导方向。

---

### 3. 和实际 diffusion 模型输出的关系

上面的公式用的是 score 形式：

$$  
s(x_t,t)=\nabla_{x_t}\log p(x_t)  
$$

但实际 diffusion 模型不一定直接预测 score。

有些模型预测噪声 $\epsilon$，有些模型预测 velocity $v$，有些模型预测 denoised sample $x_0$。

因此在实际代码或论文中，CFG 也常写成：

# $$  
\epsilon_\text{guided}

\epsilon_\text{uncond}  
+  
\gamma\left(\epsilon_\text{cond}-\epsilon_\text{uncond}\right)  
$$

或者：

# $$  
v_\text{guided}

v_\text{uncond}  
+  
\gamma\left(v_\text{cond}-v_\text{uncond}\right)  
$$

它们的直觉是一样的：

# $$  
\text{guided prediction}

## \text{unconditional prediction}  
+  
\gamma \times  
\left(  
\text{conditional prediction}

\text{unconditional prediction}  
\right)  
$$

也就是：

# $$  
\text{最终采样方向}

\text{自然生成方向}  
+  
\gamma \times \text{条件增强方向}  
$$

---

### 4. 总结

传统 **Classifier Guidance**：

$$  
\text{Diffusion model} + \text{Classifier}  
$$

它用 classifier 的梯度 $\nabla_x\log p(y\mid x)$ 来引导生成过程。

**Classifier-Free Guidance**：

$$  
\text{一个同时支持 conditional / unconditional 的 diffusion model}  
$$

它不需要额外 classifier，而是用：

$$  
s_\text{cond}-s_\text{uncond}  
$$

作为条件引导方向。

所以二者的核心区别是：

|方法|是否需要额外 classifier|条件方向来自哪里|
|---|---|---|
|Classifier Guidance|需要|$\nabla_x\log p(y\mid x)$|
|Classifier-Free Guidance|不需要|$s_\text{cond}-s_\text{uncond}$|

最简理解：

**Classifier Guidance 是用外部分类器告诉 diffusion 往哪个类别走；Classifier-Free Guidance 是让 diffusion 自己同时学会有条件和无条件预测，然后用二者差值当作条件引导方向。**。
![](../../../../pic/Pasted%20image%2020260613040346.png)