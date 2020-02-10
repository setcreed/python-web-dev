# 模块的四种形式

在python中，总共有以下四种形式的模块：

1. 内置模块：python解释器启动自带的模块，random / time ……
2. pip install 安装的模块
3. 自定义模块：如果你自己写一个py文件，在文件内写入一堆函数，则它被称为自定义模块，即使用python编写的.py文件
4. 包：把一系列模块组织到一起的文件夹（注：文件夹下有一个`__init__.py`文件，该文件夹称为包）

# import和from...import...

## import 模块

`import time`这个代码做了以下事情

- 开辟内存空间，内存空间命名为time
- 把time.py中的所有代码读入名称空间，然后运行
- 通过time.方法名使用time模块中的方法

## from 模块 import 方法

```python
from time import sleep
```

- 开辟内存空间，内存空间命名为time
- 把time.py中的所有代码读入名称空间，然后运行
- 把sleep()方法读入当前文件中，因此可以直接使用sleep方法

如果想使用多个方法：

```python
from time import sleep,time
```

导入所有功能：

```python
from time import *
```

## import与from...import的优缺点

import：

- 优点：永不冲突
- 缺点：每次导入多输入几个字符

from...import的优缺点：

- 优点：少输入几个字符
- 缺点：容易与当前执行文件中名称空间中的名字冲突