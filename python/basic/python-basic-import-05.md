# Python模块导入与使用
## I.基础内容讲解
### 1. 核心语法与场景  
```python
# 基础导入：导入整个模块
import math
print(math.sqrt(16))  # 4.0

# 别名导入：避免命名冲突
import pandas as pd

# 精确导入：仅引入所需对象
from datetime import datetime
print(datetime.now())

# 全部导入：存在命名污染风险（慎用）
from random import *
print(randint(1,10))
```

### 2. 路径搜索机制  
```python
import sys
print(sys.path)  # 查看搜索路径顺序

# 典型搜索顺序：
# 1. 当前执行文件目录
# 2. PYTHONPATH环境变量路径
# 3. Python标准库目录
# 4. site-packages第三方库目录
```

### 3. 常见陷阱  
```python
# 陷阱1：覆盖导入
from time import time
import time  # 后导入的覆盖前导入的time()

# 陷阱2：本地文件优先
# 如果存在本地requests.py文件
import requests  # 导入的是本地文件而非第三方库
```

---

## II.进阶内容补充
### 1. 模块执行机制  
```python
# 查看模块缓存
print(sys.modules.keys())  # 已加载模块字典

# 模块重载（调试时使用）
from importlib import reload
reload(some_module)  # 重新执行模块代码
```

### 2. 包管理技巧  
```python
# 相对导入（包内部使用）
from . import utils  # 同包内模块
from ..parent_pkg import config  # 上级包

# 命名空间包（Python3.3+）
# 目录结构：
# /path1/namespace/pkg/module1.py
# /path2/namespace/pkg/module2.py
# 自动合并为同一个包
```

### 3. 动态导入黑魔法  
```python
# 按需加载模块
module_name = "json" if condition else "pickle"
data_module = __import__(module_name)

# 安全动态导入（推荐）
from importlib import import_module
crypto = import_module("Crypto.Cipher.AES")
```

### 4. 模块通信模式  
```python
# 单例模式实现
# shared_config.py
config_data = {"debug": True}

# module_a.py
from shared_config import config_data
config_data["debug"] = False

# module_b.py
from shared_config import config_data
print(config_data["debug"])  # 输出False
```

---

## III. 面试问题*
### 1. 基础题  
- `import module` 与 `from module import func` 的区别？  
  答案：前者引入整个命名空间，后者直接绑定目标对象到当前命名空间

- 解释 `if __name__ == "__main__"` 的作用  
  答案：模块作为脚本直接运行时触发，被导入时不执行

### 2. 进阶题  
- 如何处理循环导入问题？  
  答案：延迟导入、重构代码结构、使用接口模块

- Python模块缓存机制如何运作？  
  答案：通过`sys.modules`字典实现，首次导入后重复导入直接返回缓存对象

### 3. 陷阱题  
    代码输出问题
    lib/__init__.py 中包含 print("INIT LIB")
    执行 import lib 是否会打印？
    答案：会输出"INIT LIB"


---

## IV. 知识扩展
### 1. 冷知识  
- `.pyc`文件作用：缓存字节码加速加载  
- 模块对象的`__file__`属性：查看模块物理路径  
```python
import os
print(os.__file__)  # 显示os模块位置
```

### 2. 最佳实践  
- 避免在`__init__.py`中放大量代码  
- 使用绝对导入代替相对导入（大型项目适用）  
- 通过`sys.path.insert(0, path)`控制搜索优先级  

### 3. 底层原理  
```python
# 手工模拟导入过程
def fake_import(module_name):
    if module_name in sys.modules:
        return sys.modules[module_name]
    # 1. 查找模块文件
    # 2. 编译字节码
    # 3. 执行模块代码
    # 4. 创建模块对象
    # 5. 加入sys.modules
    # 6. 返回模块对象
```

---

## V.实战代码测试
```python
# 问题：以下代码有什么问题？
# module_alpha.py
value = 10
def show():
    print(value)

# module_beta.py
from module_alpha import show
value = 20
show()

# 答案：输出10（模块隔离特性）
```