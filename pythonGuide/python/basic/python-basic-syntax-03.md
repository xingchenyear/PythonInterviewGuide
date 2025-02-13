# 控制流(条件/循环语句)

## I.条件语句(if/elif/else)

### 1.基础语法
```python
if condition1:
    # 代码块
elif condition2:
    # 代码块
else:
    # 默认代码块
```

### 2.深度特性

- **短路求值**：Python使用惰性求值，遇到第一个True条件即停止

```python
# 示例：安全访问字典
value = d['key'] if 'key' in d else None
```

- **布尔上下文**：所有对象都有隐式布尔值(None, 0, 空容器为False)

 
```python
# 判断列表是否非空的更优雅写法
if my_list:  # 优于 if len(my_list) > 0
    process_list()
```

3. **进阶技巧**
- **三元表达式**（面试高频）
```python
max_value = a if a > b else b
```

- **链式比较**（Python特有）
```python
if 0 < x < 10:  # 等价于 x>0 and x<10
    print("Valid value")
```

---

## II.循环语句
1. **for循环**
```python
for item in iterable:
    # 处理item
```

**关键知识点**：
- `range`函数生成迭代器（Python3特性）
```python
for i in range(5):      # 0-4
for i in range(1,6):    # 1-5
for i in range(0,10,2): # 0,2,4,6,8
```

- **遍历时修改集合的注意事项**
```python
# 错误示范（遍历时删除元素）
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # 会导致意外跳过元素

# 正确做法
numbers = [num for num in numbers if num % 2 != 0]
```

2. **while循环**
```python
while condition:
    # 循环体
```

3. **循环控制语句**
- `break`：立即终止整个循环
- `continue`：跳过本次迭代
- `else`子句（Python特有）：循环正常结束（非break退出）时执行
```python
for n in primes:
    if n % target == 0:
        print("Found divisor")
        break
else:
    print("No divisors found")  # 循环未找到时执行
```

---

## III. 高级迭代技巧
1. **enumerate**：获取索引和值
```python
for idx, value in enumerate(['a', 'b', 'c'], start=1):
    print(f"Index {idx}: {value}")
```

2. **zip**：并行迭代多个序列
```python
names = ['Alice', 'Bob']
scores = [95, 88]
for name, score in zip(names, scores):
    print(f"{name}: {score}")
```

3. **列表推导式 vs 生成器表达式**
```python
# 列表推导式（立即计算）
squares = [x**2 for x in range(10)]

# 生成器表达式（惰性计算）
squares_gen = (x**2 for x in range(10))
```

---

## IV.常见面试问题
1. **如何避免循环中的无限循环？**
   - 确保while条件最终会变为False
   - 设置循环次数上限
   - 使用timeout机制

2. **解释循环中else块何时执行**
   - 当循环正常完成（非break退出）时触发
   - 常见应用场景：搜索场景的"未找到"处理

3. **如何实现带索引的循环？**
   - 优先使用enumerate而非range(len())
   ```python
   # 反模式
   for i in range(len(my_list)):
       print(i, my_list[i])
   
   # 正确方式
   for i, item in enumerate(my_list):
       print(i, item)
   ```

4. **迭代器与可迭代对象的区别**
   - 可迭代对象：实现了`__iter__()`方法（如list, tuple）
   - 迭代器：实现了`__next__()`方法，保存迭代状态

---

## V. 进阶知识扩展

1. 避免在循环内重复计算固定值

```python
# 差实践
for i in range(len(big_list)):
    result = process(big_list[i], len(big_list))

# 改进方案
total = len(big_list)
for i in range(total):
    result = process(big_list[i], total)
```