# gevent

## gevent模块介绍

gevent是一个第三方模块，可以帮你监听IO操作，并切换

使用gevent的目的：在单线程下实现，遇到IO就会 保存状态 + 切换



## gevent使用

```python
import time
from gevent import monkey

monkey.patch_all()  # 可以监听该程序下所有的IO操作
from gevent import spawn, joinall # 用于做切换 + 保存状态


def func1():
    print('1')
    time.sleep(1)  # IO操作


def func2():
    print('2')
    time.sleep(3)


def func3():
    print('3')
    time.sleep(5)

start = time.time()

s1 = spawn(func1)
s2 = spawn(func2)
s3 = spawn(func3)

s1.join()  # 发送信号，相当于等待自己（在单线程的情况下）
s2.join()
s3.join()

# joinall((s1, s2, s3))  #  一个个执行很麻烦，可以用joinall把这些全部装进去

end = time.time()
print(end - start)  # 5.006161451339722
```