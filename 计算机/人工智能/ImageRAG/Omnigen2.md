encode_prompt
1. 会将prompt分为
	- 正向提示嵌入- 指导模型生成想要的内容
	- 负向提示嵌入- 告诉模型避免生成的内容
2. 调用Qwen2.5-VL 多模态语言模型将文本转换为隐向量
3. 为每个 prompt 的多个生成进行复制
4. 同样处理负向提示
5. 返回四元组
(
   prompt_embeds: (4, 256, 768),              # 正向提示嵌入
   prompt_attention_mask: (4, 256),          # 正向提示掩码
   negative_prompt_embeds: (4, 256, 768),    # 负向提示嵌入
   negative_prompt_attention_mask: (4, 256)  # 负向提示掩码
)