# instance

python内置函数，可以传入两个参数，用于判断参数1是否是参数2的一个实例。

判断一个对象是否是一个类的实例

```python
class Foo:
    pass

foo_obj = Foo()
print(foo_obj)
print(foo_obj.__class__.__dict__)
print(foo_obj in Foo.__dict__)   # 判断一个对象是否是类的实例

print(isinstance(foo_obj, Foo))  # 判断一个对象是否是类的实例
```

# issubclass

python内置函数，可以传入两个参数，用于判断参数1是否是参数2的一个子类

判断一个类是否是一个类的子类

```python
class Foo:
    pass


class Goo(Foo):
    pass


print(issubclass(Goo, Foo))  # True
```