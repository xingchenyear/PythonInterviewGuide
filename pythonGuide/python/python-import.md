# Python 引用

## Python 弱引用

在 Python 中的引用默认是强引用,弱引用（Weak References）是一种特殊的引用方式,它不会增加对象的引用计数。这意味着当一个对象除了弱引用之外没有其他强引用时,该对象可以被垃圾回收。弱引用常用于避免循环引用导致的内存泄漏,以及在需要缓存对象但又不希望阻止对象被回收的情况下使用。

### 弱引用的应用场景

1. 缓存：

   * 内存管理：在缓存中使用弱引用可以确保缓存的对象在不再被其他地方使用时能够被回收，从而避免内存泄漏。
   * 示例：缓存数据库查询结果或计算结果，但不希望这些缓存对象阻止原始对象的回收。

2. 观察者模式：

   * 避免循环引用：在实现观察者模式时，使用弱引用来存储观察者，可以避免观察者和被观察者之间的循环引用，从而确保对象能够被正确回收。
   * 示例：GUI 应用中，窗口对象作为观察者观察数据模型的变化，使用弱引用来存储观察者。

3. 事件处理：

   * 动态对象管理：在事件处理系统中，使用弱引用来存储事件处理函数或对象，可以确保这些对象在不再需要时能够被回收。
   * 示例：事件驱动的框架中，动态注册和注销事件处理函数。

4. 元数据存储：

   * 辅助信息：存储与对象相关的元数据，但不希望这些元数据阻止对象的回收。
   * 示例：存储对象的调试信息或日志信息。

### 弱引用使用方法

Python 的 weakref 模块提供了创建和管理弱引用的功能

#### 示例

1.创建弱引用
~~~python
import weakref

class MyClass:
 def __init__(self, value):
     self.value = value

obj = MyClass(10)
weak_obj = weakref.ref(obj)
print(weak_obj())  # 输出: <__main__.MyClass object at 0x...>
del obj
print(weak_obj())  # 输出: None
~~~ 

2.弱引用回调函数
~~~python
import weakref

def callback(reference):
 print("Object has been garbage collected")

class MyClass:
 def __init__(self, value):
     self.value = value

obj = MyClass(10)
weak_obj = weakref.ref(obj, callback)
del obj
# 输出: Object has been garbage collected
~~~


3.弱引用的集合

使用 weakref.WeakSet 或 weakref.WeakKeyDictionary 来存储多个弱引用。

~~~python
#   WeakSet：存储弱引用的对象集合。

import weakref

class MyClass:
 def __init__(self, value):
     self.value = value

obj1 = MyClass(10)
obj2 = MyClass(20)

weak_set = weakref.WeakSet([obj1, obj2])
print(list(weak_set))  # 输出: [<__main__.MyClass object at 0x...>, <__main__.MyClass object at 0x...>]

del obj1
print(list(weak_set))  # 输出: [<__main__.MyClass object at 0x...>]
     
~~~

~~~python
#   WeakKeyDictionary：键为弱引用的对象，值为任意对象。
import weakref

class MyClass:
    pass

obj1 = MyClass()
obj2 = MyClass()

weak_dict = weakref.WeakKeyDictionary()
weak_dict[obj1] = 'value1'
weak_dict[obj2] = 'value2'

print(list(weak_dict.keys()))  # 输出: [<__main__.MyClass object at 0x...>, <__main__.MyClass object at 0x...>]

del obj1
print(list(weak_dict.keys()))  # 输出: [<__main__.MyClass object at 0x...>]，因为 obj1 被删除，弱引用被移除

~~~

## 循环引用


### Python 循环import引用


### Python 对象循环引用
