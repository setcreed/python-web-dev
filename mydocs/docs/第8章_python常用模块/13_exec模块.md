# exec模块的补充

exec的作用：

可以把“字符串形式”的python代码，添加到全局空间或局部空间中

使用（传入三个参数）：

- 参数一：“字符串形式”的python代码
- 参数二：全局名称空间
- 参数三：局部名称空间

```python
# 全局

# 1.文本形式的python代码
code = '''
global x
x = 10
y = 20
'''

# 2.全局名称空间
global_dict = {'x': 200}
# 3.局部名称空间
local_dict = {}

exec(code, global_dict, local_dict)

print(global_dict)

```

```python
# 1.文本形式的python代码
code = '''
x = 100
y = 200

def func():
    pass

'''

# 2.全局名称空间
global_dict = {}
# 3.局部名称空间
local_dict = {}

exec(code, global_dict, local_dict)

print(local_dict)
```