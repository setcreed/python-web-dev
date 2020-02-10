# typing模块

与函数联用，控制函数参数的数据类型，提供了基础数据类型之外的数据类型

```python
from typing import Iterable

def func(x:int, lt:list) -> list:
    return [1,2,3]

func(10,[12,3])
```

## typing模块作用

- 类型检查，防止运行时出现参数和返回值类型不符合。
- 作为开发文档附加说明，方便使用者调用时传入和返回参数类型。
- 该模块加入后并不会影响程序的运行，不会报正式的错误，只有提醒。
- 注意：typing模块只有在python3.5以上的版本中才可以使用,pycharm目前支持typing检查

## typing常用类型

- int、long、float: 整型、长整形、浮点型
- bool、str: 布尔型、字符串类型
- List、 Tuple、 Dict、 Set:列表、元组、字典、集合
- Iterable、Iterator:可迭代类型、迭代器类型
- Generator：生成器类型