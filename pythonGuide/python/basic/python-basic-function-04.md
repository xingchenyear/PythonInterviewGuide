
# Python函数

Python的"一切皆对象"函数也是对象
## I. 函数基础
### 1. 函数定义
```python
def function_name(parameters):
    """函数文档字符串（可选）"""
    # 函数体
    return value  # 可选
```
- **缩进规则**：函数体必须缩进（通常4空格）
- **函数命名**：推荐snake_case命名，需见名知意
- **文档字符串**：用三引号编写，可通过`__doc__`访问

### 2.参数类型（高频考点）
| 参数类型       | 示例                 | 说明                          |
|----------------|----------------------|-----------------------------|
| 位置参数       | `def func(a, b)`     | 按顺序传递                   |
| 默认参数       | `def func(a=1)`      | 必须放在位置参数之后          |
| 可变位置参数   | `def func(*args)`    | 接收元组，可处理不定长参数    |
| 可变关键字参数 | `def func(**kwargs)` | 接收字典，处理键值对参数      |
| 仅关键字参数   | `def func(*, a)`     | 调用时必须使用关键字参数指定 |

**参数顺序规则**：  
位置参数 -> 默认参数 -> *args -> 仅关键字参数 -> **kwargs

### 3.返回值特性
- 可返回多个值（实际返回元组）：
```python
def get_coordinates():
    return 10, 20  # 等价于 return (10, 20)
x, y = get_coordinates()
```
- 无return语句时返回`None`
- 函数可作为返回值（高阶函数特性）

### 4.函数调用方式
```python
# 位置参数调用
add(2, 3)

# 关键字参数调用
add(a=2, b=3)

# 混合调用（位置参数在前）
add(2, b=3)

# 解包参数
args = (1, 2, 3)
kwargs = {'x': 10, 'y': 20}
func(*args, **kwargs)
```

### 5. 作用域与闭包
- **LEGB规则**：
  1. Local（局部作用域）
  2. Enclosing（闭包函数外的函数）
  3. Global（模块全局）
  4. Built-in（内建作用域）

- **global/nonlocal声明**：
```python
x = 10
def outer():
    y = 20
    def inner():
        nonlocal y  # 修改闭包变量
        global x    # 修改全局变量
        x += 1
        y += 1
```

## II.高阶函数特性

### 1.函数作为参数
```python
def apply(func, value):
    return func(value)

apply(lambda x: x**2, 5)  # 返回25
```

### 2.闭包经典模式
```python
def make_adder(n):
    def adder(x):
        return x + n
    return adder

add5 = make_adder(5)
add5(3)  # 返回8
```

### 3. Lambda表达式
```python
square = lambda x: x**2
sorted(list, key=lambda x: x[1])  # 按第二个元素排序
```
- 只能包含单个表达式
- 没有语句（不能包含return/if等）

## III.常见面试问题
1.可变默认参数问题

```python
# 错误示例
def append_to(element, target=[]):
    target.append(element)
    return target

# 正确写法
def append_to(element, target=None):
    if target is None:
        target = []
    target.append(element)
    return target
```

2.变量作用域混淆

```python
x = 10
def func():
    print(x)  # 报错！因为后面有x赋值，Python认为x是局部变量
    x = 20
```

3.参数传递实质

Python采用"按对象引用传递"，对于可变对象

```python
def modify(lst):
    lst.append(4)
    
a = [1,2,3]
modify(a)  # a会被修改为[1,2,3,4]
```

4. 解释`*args`和`**kwargs`的区别?
    * `*args` 是个元组 `**kwargs`是字典

5. 如何实现函数重载？（Python如何实现多态？）
    * Python原生不支持传统OOP语言的重载方式，可以用方法参数来实现类似的功能

6. 函数装饰器的实现原理是什么？
    * 装饰器的实现原埋是基于函数作为一等对象，可以被传递和返回，以及闭包的使用来保存上下文。装饰器本质上是一个高阶函数，它接受
个函数作为输入，返回一个新的函数，增强了原函数的功能。

7. 解释闭包中的变量捕获机制？
    * 闭包捕获的是变量的引用，而非当前值的副本。变量的最终值取决于闭包被调用时变量的状态，而非定义时的值。

