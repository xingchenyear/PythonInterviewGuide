### **模块导入与使用**  
（模块机制体现了Python的模块化编程思想，深入理解其机制是面试重点）

---

#### **1. 基础语法**
```python
# 基础导入
import math
print(math.sqrt(9))  # 3.0

# 导入特定内容
from os import path
print(path.abspath('.'))

# 别名导入
import numpy as np
np.array([1,2,3])

# 导入全部内容（慎用）
from requests import *
```

---

#### **2. 导入路径与搜索顺序**
- **搜索顺序**：  
  1. 当前执行脚本目录  
  2. PYTHONPATH环境变量指定路径  
  3. 标准库安装目录  
  4. site-packages第三方库目录  

```python
# 查看模块搜索路径
import sys
print(sys.path)

# 动态添加路径
sys.path.append("/custom/path")
```

---

#### **3. 常见问题与陷阱**
**（1）循环导入问题**  
```python
# module_a.py
import module_b  # 报错：循环引用

# 解决方案：延迟导入（在函数内部import）
def func():
    import module_b
    # ...
```

**（2）模块重载问题**  
```python
import importlib
import my_module

# 修改模块后重新加载
importlib.reload(my_module)
```

**（3）动态导入**  
```python
# 根据配置加载不同模块
module_name = "json"
data_handler = __import__(module_name)

# 更规范的动态导入方式
from importlib import import_module
csv_parser = import_module("csv")
```

---

#### **4. 模块的执行与保护**
```python
# module_guard.py
if __name__ == "__main__":
    # 仅当直接运行时执行的代码
    test_code()

# 被导入时不会执行测试代码
```

---

#### **5. 高级技巧**
- **相对导入**（包内部使用）：
```python
# 在包内部的module中使用
from . import sibling_module
from ..parent_module import func
```

- **命名空间包**（Python 3.3+）：
```python
# 跨目录组织包
# 目录结构：
# /path1/pkg/module1.py
# /path2/pkg/module2.py
# 可以同时导入为 pkg 包的两个模块
```

---

#### **6. 高频面试问题**
1. `import module` vs `from module import func` 的性能差异？
   - 答案：首次导入时性能相同，后续使用时`from...import`略快

2. 如何处理模块中的全局变量？
   - 答案：模块级变量本质是模块对象的属性，不同导入方式共享同一实例

3. Python如何缓存已导入的模块？
   - 答案：存放在`sys.modules`字典中，重复导入直接取出

4. `__init__.py`文件的作用？
   - 答案：标识包目录，可包含包初始化代码（Python3.3+后非必须）

---

#### **7. 代码陷阱示例**
```python
# 问题代码：导入顺序导致的覆盖问题
from numpy import datetime
import datetime  # 覆盖了前面的导入

# 问题代码：同名模块冲突
# 存在本地文件socket.py时
import socket  # 优先导入本地文件而非标准库
```

---

#### **面试题示例**
```python
# 问题：以下代码的输出是什么？
# module1.py
print("Initializing module1")

# module2.py
import module1
import module1

# 执行python module2.py
# 答案：只打印一次"Initializing module1"
``` 

---

这份补充内容涵盖了模块机制的核心知识点，既包含基础语法，也涉及高频面试问题与易错点，能有效考察候选人对Python模块系统的理解深度。