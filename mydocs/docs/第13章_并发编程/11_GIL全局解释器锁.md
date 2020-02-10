# GIL全局解释锁

Python代码的执行由Python虚拟机(也叫解释器主循环)来控制。Python在设计之初就考虑到要在主循环中，同时只有一个线程在执行。虽然 Python 解释器中可以“运行”多个线程，但在任意时刻只有一个线程在解释器中运行。

对Python虚拟机的访问由全局解释器锁(GIL)来控制，正是这个锁能保证同一时刻只有一个线程在运行。

全局解释锁特点：

1. GIL本质上是一个互斥锁。
2. GIL是为了阻止同一个进程内多个进程同时执行(并行)
   - 单个进程下的多个线程无法实现并行,但能实现并发
3. 这把锁主要是因为Cpython的内存管理不是线程安全的
   - 保证线程在执行任务时不会被垃圾回收机制回收

```python
from threading import Thread
import time

num = 100


def task():
    global num
    num2 = num
    time.sleep(1)
    num = num2 - 1
    print(num)


for line in range(100):
    t = Thread(target=task)
    t.start()
    
# 这里的运行结果都是99, 加了IO操作,所有线程都对num进行了减值操作,由于GIL锁的存在,没有修改成功,都是99
```



在多线程环境中，Python 虚拟机按以下方式执行：

1. 设置 GIL；
2. 切换到一个线程去运行；
3. 运行指定数量的字节码指令或者线程主动让出控制(可以调用 time.sleep(0))；
4. 把线程设置为睡眠状态；
5. 解锁 GIL；
6. 再次重复以上所有步骤。