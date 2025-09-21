---
aliases:
  - unicode
---
![](../../pic/Pasted%20image%2020250921173701.png)
- UTF-8是变长编码
| Unicode编码范围（十六进制） | UTF-8编码方式（二进制） |
| --- | --- |
| 000000 - 00007F | 0xxxxxxx（ASCII编码） |
| 000080 - 0007FF | 110xxxxx 10xxxxxx |
| 000800 - 00FFFF | 1110xxxx 10xxxxxx 10xxxxxx |
| 010000 - 10FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |