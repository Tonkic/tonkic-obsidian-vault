---

---
- 有序（ordered）：元素有固定位置，可以通过索引访问。
- 不可变（immutable）：创建后不能修改（不能增删改元素），这与 list 不同。
- 可以包含任意类型的对象，并且可以嵌套（元素可以是 list、tuple、dict 等）。
- 如果元组内所有元素都可哈希（hashable），整个元组也是可哈希的，可以用作字典的键或集合的元素。

```python
# 创建
t = (1, 2, 3)
print(t[0])        # 输出 1，索引从0开始
print(len(t))      # 输出 3

# 不可变：下面会报错
# t[0] = 10  # TypeError: 'tuple' object does not support item assignment

# 解包（unpacking）
a, b, c = t
print(a, b, c)     # 1 2 3

# 交换变量值（常用技巧）
a, b = b, a

# 切片和拼接（返回新的元组）
print(t[1:])       # (2, 3)
t2 = t + (4, 5)
print(t2)          # (1, 2, 3, 4, 5)

# 转换
lst = list(t)      # tuple -> list
t_from_list = tuple([7, 8, 9])  # list -> tuple

# 可作为字典键（前提：元组中元素可哈希）
d = {}
d[(1, 2)] = "pair"
print(d[(1, 2)])   # "pair"
```