---
<<<<<<< HEAD
aliases:
  - unicode
---
![](../../pic/Pasted%20image%2020250921173701.png)
=======
tags:
  - 计算机
  - python
  - UTF-8
---
---
aliases:
  - unicode
---
![[../../pic/Pasted image 20250921173701.png]]
>>>>>>> tonkic-obsidian-vault/main
- UTF-8是变长编码

| Unicode编码范围（十六进制） | UTF-8编码方式（二进制） |
| --- | --- |
| 000000 - 00007F | 0xxxxxxx（ASCII编码） |
| 000080 - 0007FF | 110xxxxx 10xxxxxx |
| 000800 - 00FFFF | 1110xxxx 10xxxxxx 10xxxxxx |
| 010000 - 10FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |
- 比如中（20013），转换为0100 111000 101101 填入三字节的编码方式中的xxx
- 位数不足的补零

python用法：
```python
ord('牛') -> 29275
chr(29275) -> '牛'
test_string = "hello! こんにちは!"
utf8_encoded = test_string.encode("utf-8") #返回bytes，表现为b'...'（如 b'hello'在ASCII 范围保持可读，b'\xe5\xa5\xbd'其他不可读）
print(utf8_encoded.decode("utf-8"))
```
