# random模块

```python
import random

# 0-1随机数
print(random.random())


# 1-100随机整数
print(random.randint(1,100))

# 1-3之间随机整数
print(random.randrange(1, 3))

# 打乱lt的顺序
lt = [1,2,4,60]
random.shuffle(lt)   # [4, 1, 60, 2]
print(lt)

# 随机选择lt中一个元素
print(random.choice(lt))

# random.seed
import random

random.seed(4)   # 给一个随机数种子
print(random.random())   # 只第一次随机生成，之后生成的数字就一样了
print(random.random())

# 如果不自定义种子，则种子按照当前的时间来
```



# 计算圆周率

- 公式法计算

圆周率计算公式：

$$
\pi = \sum_{k=0}^\infty [\frac{1}{16^k} (\frac{4}{8k+1}-\frac{2}{8k+4}-\frac{1}{8k+5}-\frac{1}{8k+6})]
$$



```python
pi = 0
k = 0
while True:
    pi += (1 / (16 ** k)) * (4 / (8 * k + 1) - 2 / (8 * k + 4) - 1 / (8 * k + 5) - 1 / (8 * k + 6))

    print(pi)
    k += 1
```

- 蒙特卡罗方法计算圆周率

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131205330.png)

```python
import random

count = 0
for i in range(1000000):
    x, y = random.random(), random.random()
    distance = pow(x**2 + y**2, 0.5)
    if distance < 1:
        count += 1
print(count/1000000*4)
```