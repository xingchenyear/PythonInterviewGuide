# Python数据类型

## I. 基本数据类型


    数值类型、布尔类型、空类型、容器类型、特殊类型等。

###  1.数值类型
```python

num_int = 42        # int
num_float = 3.14    # float
complex_num = 1+2j  # complex


# 布尔类型
is_valid = True     # bool

# 空类型
empty = None        # NoneType
```

### 2. 容器类型

```python

str_var = "hello"   # str (不可变序列)
list_var = [1,2]    # list (可变序列)
tuple_var = (1,2)   # tuple (不可变序列)
dict_var = {"a":1}  # dict (可变映射）
# 集合
set_var = {1,2}     # set (可变集合)
frozenset_var = frozenset({1,2}) # frozenset (不可变集合)

```

### 3. 特殊类型

```python
import math
def func(): pass     # function
module = math        # module
class MyClass: pass  # class

```

---

## II. 类型检查机制
    如何查看当前类型是什么？以及如何判断类型是否是一样的？

#### 1. type()函数

```python
print(type(5))              # <class 'int'>
print(type([1,2]))          # <class 'list'>
print(type(3) == int)       # True
```

#### 2. isinstance()函数

```python
print(isinstance(5, int))   # True
print(isinstance(5, object))# True (所有类的基类)
```

#### 3. 类型检查原则
- 优先使用`isinstance()`检查类型（考虑继承关系）
- `type()`严格检查类型一致性

---

## III. 可变与不可变类型

#### 1. 不可变类型（Immutable）

```python
# 数值类型
x = 5
print(id(x))  # 140735212039432
x += 1
print(id(x))  # 内存地址变化

# 字符串
s = "hello"
print(id(s))  # 2101452341232
s += "!"
print(id(s))  # 新内存地址

```

#### 2. 可变类型（Mutable）
```python
lst = [1,2]
print(id(lst))  # 2101452345472
lst.append(3)
print(id(lst))  # 内存地址不变

```
#### 3.特殊案例

    在元组中包含可变元素是可以的并且可以操作可变元素。

```python

# 元组包含可变元素 
t = ([1,2], 3)
t[0].append(3)  # 合法操作

```

| 可变类型  | 不可变类型     |
|-------|-----------|
| list  | int/float |
| dict  | str       |
| set   | tuple     |
| 自定义对象 | bool      |
|       | frozenset |


#### 4. 区别
| **特性**      | 不可变类型   | 可变类型   |
|-------------|---------|--------|
| **内存变化**    | 修改创建新对象 | 原地修改   |
| **哈希支持**    | 可哈希     | 不可哈希   |
| **默认参数安全性** | 安全      | 需要特殊处理 |
| **线程安全**    | 天然安全    | 需要同步机制 |

---

## IV. 拷贝机制
#### 1. 赋值与拷贝的区别

```python
import copy
a = [1, [2,3]]
b = a          # 赋值（引用传递）
c = a.copy()   # 浅拷贝
d = copy.deepcopy(a)  # 深拷贝
```

#### 2. 浅拷贝场景
```python
original = [1, [2,3]]
shallow_copy = original.copy()

shallow_copy[0] = 100  # 不影响原列表
print(original)        # [1, [2,3]]

shallow_copy[1][0] = 200  # 修改嵌套对象
print(original)           # [1, [200,3]]
```

#### 3. 深拷贝实现
```python
import copy

original = [1, [2,3]]
deep_copy = copy.deepcopy(original)

deep_copy[1][0] = 200
print(original)  # [1, [2,3]] 保持原样
```

---

## V. 高频面试问题
1. `a += b` 与 `a = a + b` 对于可变对象的区别？
   * `a += b` 是原地修改，`a = a + b` 是创建新对象
2. 为什么函数默认参数推荐使用不可变类型？
    * 可变对象在函数中作为默认参数时，每次调用函数都会创建新的对象，所以不推荐使用可变对象作为默认参数
3. 如何实现自定义对象的深拷贝？
    * 使用`copy.deepcopy()`函数实现
4. 解释整数池（-5~256）现象及其影响?
    * Python整数池，在-5~256范围内，整数对象会共享同一个内存地址，避免重复创建对象，从而提高内存利用率和性能。
5. 元组的不可变性是否绝对？
    * 元组不可变性是绝对的，但元组中的可变对象是可以修改的。

---

## VI. 进阶知识扩展
1. **内存优化机制**：字符串驻留、小整数池
2. **魔术方法**：`__hash__`、`__eq__`与不可变性的关系
3. **协变类型**：`list[float]`是否可以作为`list[Number]`使用
4. **结构模式匹配**（Python 3.10+）：类型在模式匹配中的应用

