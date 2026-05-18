### 开源
https://github.com/MinghoKwok/MemEye

### 简介
一个专门评测“多模态智能体长期记忆”的 benchmark / 实验框架。它不是一个新模型，而是用来测试各种 memory 方法到底能不能把“视觉信息”记住，并在之后正确检索、推理和更新。

### 实验结果
|排名|方法|类型|记忆能力判断|
|---|---|---|---|
|1|**SRAG(V)**|多模态语义 RAG|综合最强|
|2|**FC(V)**|多模态 full context|整体较强，尤其粗粒度和状态题还可以|
|3|**MMA**|多模态记忆 agent|细粒度视觉细节较强|
|4|**FC(T)**|文本 full context|意外地强，尤其时间状态问题|
|5|**SRAG(T)**|文本语义 RAG|选择题还不错，但开放回答弱于 SRAG(V)|
|6|**MemOS / A-Mem / Reflexion / M2A**|结构化或 agentic memory|局部场景有优势，但不稳定|
