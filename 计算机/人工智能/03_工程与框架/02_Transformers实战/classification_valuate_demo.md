```python
def evaluate():
    model.eval() #评估模式，关闭dropout等
    acc_num = 0 #累计正确数量
    with torch.inference_mode():
        for batch in validloader:
            if torch.cuda.is_available():
                batch = {k: v.cuda() for k, v in batch.items()} 
                #k全部不变，v全部进cuda
            output = model(**batch) 
            #解包batch，等价于model(input_ids=batch["input_ids"], attention_mask=batch["attention_mask"], labels=batch["labels"])
            pred = torch.argmax(output.logits, dim=-1) 
            #最后一维度取logits概率最大值作为分类结果
            acc_num += (pred.long() == batch["labels"].long()).float().sum()
    return acc_num / len(validset)
```