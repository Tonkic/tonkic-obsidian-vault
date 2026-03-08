---
tags:
  - 计算机
  - python
  - collections
---
dict 的子类，用来把“可哈希对象”映射到整数计数（frequency）。常用于统计元素出现次数（词频、bigram 频次等）。
- Counter(iterable) —— 例如 Counter("aab") -> Counter({'a':2,'b':1})
- Counter(mapping) —— 例如 Counter({'a':2,'b':1})
- Counter(\*\*kwargs) —— 例如 Counter(a=2, b=1)