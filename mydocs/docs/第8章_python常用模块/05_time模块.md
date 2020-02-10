# time模块

## 时间戳

```python
import time

print(time.time())   # 从1970年1月1日00:00:00开始计算到现在的秒数
```

## 格式化时间

```python
import time 
print(time.strftime('%Y-%m-%d %H:%M:%S'))  # 2019-09-28 17:15:47

print(time.strftime('%Y-%m-%d %X'))   # 2019-09-28 17:16:50
```

## 结构化时间

```python
import time
print(time.localtime())   # time.struct_time(tm_year=2019, tm_mon=9, tm_mday=28, tm_hour=17, tm_min=18, tm_sec=11, tm_wday=5, tm_yday=271, tm_isdst=0)
# 结构化基础时间
import time
print(time.localtime(0))   # time.struct_time(tm_year=1970, tm_mon=1, tm_mday=1, tm_hour=8, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=1, tm_isdst=0)
```

## sleep

```python
import time 

start = time.time()
time.sleep(2)
end = time.time()
print(f'暂停了{end - start}秒')  # 暂停了2.000108003616333秒
```



# 文本进度条展示

```python
import time

print(time.time())  # 从1970.01.01.00:00开始计算时间
import time

print('-------')
time.sleep(3)  # 睡眠
print('-------')
# cpu级别的时间计算，一般用于程序耗时时间计算
import time

start = time.perf_counter()
for i in range(10):
    print(i)
    time.sleep(0.01)
print(time.perf_counter() - start)

# 打印结果：
0
1
2
3
4
5
6
7
8
9
0.10681829999999998
```

## 文本进度条

```python
'''
 0 %[->..........]
10 %[*->.........]
20 %[**->........]
30 %[***->.......]
40 %[****->......]
50 %[*****->.....]
60 %[******->....]
70 %[*******->...]
80 %[********->..]
90 %[*********->.]
100%[**********->]
'''
```

## 简单开始

星号在递增，小点在递减，用两个循环

```python
for i in range(10):
    print('*'* i + '.' * (10 - i))

# 打印结果：
..........
*.........
**........
***.......
****......
*****.....
******....
*******...
********..
*********.
for i in range(10):
    print(f'[{"*" * i} -> {"." * (10 - i)}]')
    
 # 打印结果：
[ -> ..........]
[* -> .........]
[** -> ........]
[*** -> .......]
[**** -> ......]
[***** -> .....]
[****** -> ....]
[******* -> ...]
[******** -> ..]
[********* -> .]
for i in range(10):
    print(f'{i*10: ^3}% [{"*" * i} -> {"." * (10 - i)}]')
    
# 打印结果：
 0 % [ -> ..........]
10 % [* -> .........]
20 % [** -> ........]
30 % [*** -> .......]
40 % [**** -> ......]
50 % [***** -> .....]
60 % [****** -> ....]
70 % [******* -> ...]
80 % [******** -> ..]
90 % [********* -> .]
```

## 继续修改

```python
scale = 11
for i in range(scale):
    print(f'{(i/scale)*scale: ^3.1f}% [{"*" * i} -> {"." * (scale - i)}]')
    
# 打印结果：
0.0% [ -> ...........]
1.0% [* -> ..........]
2.0% [** -> .........]
3.0% [*** -> ........]
4.0% [**** -> .......]
5.0% [***** -> ......]
6.0% [****** -> .....]
7.0% [******* -> ....]
8.0% [******** -> ...]
9.0% [********* -> ..]
10.0% [********** -> .]
```

## 单条显示

```python
scale = 101
for i in range(scale):
    print(f'\r{(i/scale)*scale: ^3.1f}% [{"*" * i} -> {"." * (scale - i)}]', end='')
    
# 打印结果：
100.0% [**************************************************************************************************** -> .]
```

## 文本进度条最终形式

```python
import time

start = time.perf_counter()
scale = 101
for i in range(scale):
    print(f'\r{(i / scale) * scale: ^3.1f}% [{"*" * i} -> {"." * (scale - i)}] {time.perf_counter() - start:.2f}s',
          end='')
    time.sleep(0.1)
```