# property

## 什么是property

python内置的装饰器，主要是给类内部的方法使用

## 为什么要用property

使用它的目的，是将类内部的方法（def 方法名() 变成了（def 方法））

在对象使用某个方法时，将对象.方法()变成了对象.方法

## 如何使用property

```python
'''
计算人的bmi：bmi值 = 体重 / （身高 * 身高）
'''

class People:
    def __init__(self, name, weight, height):
        self.name = name
        self.weight = weight
        self.height = height

    @property
    def bmi(self):
        return self.weight / (self.height ** 2)

p = People('cwz', 180, 1.8)
print(p.bmi)
```

注意：**不能对被装饰过的方法属性修改**

但是也可以修改（了解）

```python
class People:
    def __init__(self, name, weight, height):
        self.name = name
        self.weight = weight
        self.height = height

    @property
    def bmi(self):
        return self.weight / (self.height ** 2)

    @property
    def get_name(self):
        return self.name

    # 修改
    @get_name.setter
    def set_name(self, val):
        self.name = val

    # 删除
    @get_name.deleter
    def del_name(self):
        del self.name


p = People('cwz', 180, 1.8)
# print(p.bmi)

p.set_name = 'nick'
# print(p.get_name)

del p.del_name

# print(p.get_name)
```