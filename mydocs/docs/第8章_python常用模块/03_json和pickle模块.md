# json和pickle模块

## 序列化和反序列化

- 序列化：按照特定的规则排列，把python数据类型转化为json串，便于跨平台传输
- 反序列化：把json串转化为python / java / c / php 需要的语言

Json序列化并不是python独有的，json序列化在java等语言中也会涉及到，因此使用json序列化能够达到跨平台传输数据的目的。

json数据类型和python数据类型对应关系表

|  Json类型  | Python类型 |
| :--------: | :--------: |
|     {}     |    dict    |
|     []     |    list    |
|  "string"  |    str     |
|   520.13   | int或float |
| true/false | True/False |
|    null    |    None    |

## json模块

```python
dic = {'a': 1, 'b': 2, 'c': None}

data = json.dumps(dic)   # json串中没有单引号
print(type(data), data)
data = json.loads(data)
print(type(data), data)

# 打印结果：
'''
<class 'str'> {"a": 1, "b": 2, "c": null}
<class 'dict'> {'a': 1, 'b': 2, 'c': None}
'''
dic = {'a': 1, 'b': 2, 'c': None}

# 序列化字典为json串，并保存文件
with open('test.json', 'w', encoding='utf-8') as fw:
    json.dump(dic, fw)
    
# 反序列化
with open(f'{"test"}.json', 'r', encoding='utf-8') as fr:
    data = json.load(fr)
    print(type(data), data)  # <class 'dict'> {'a': 1, 'b': 2, 'c': None}
```

## pickle

Pickle序列化和所有其他编程语言特有的序列化问题一样，它只能用于Python。但是pickle的好处是可以存储Python中的所有的数据类型，包括对象，而json不可以。

```python
import pickle

se = {1, 3, 4, 5, 6}
with open('test.pkl', 'wb') as fw:
    pickle.dump(se, fw)
se = {1, 3, 4, 5, 6}

def func():
    x = 3
    def wrapper():
        print(x)
    return wrapper

with open('test.pkl', 'wb') as fw:
    pickle.dump(func, fw)

with open('test.pkl', 'rb') as fr:
    data = pickle.load(fr)
    # print(data)
    res = data()
    res()
```