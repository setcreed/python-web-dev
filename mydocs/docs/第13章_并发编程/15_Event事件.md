# Event事件

用来控制线程的执行

出现`e.wait()`，就会把这个线程设置为False，就不能执行这个任务；

只要有一个线程出现`e.set()`，就会告诉Event对象，把有`e.wait`的用户全部改为True，剩余的任务就会立马去执行。由一些线程去控制另一些线程，中间通过Event。

```python
from threading import Event
from threading import Thread
import time
# 调用Event实例化出对象
e = Event()
#
# # 若该方法出现在任务中，则为False，阻塞
# e.wait()  # False
# # 若该方法出现在任务中，则将其他线程的False改为True，进入就绪态和运行态
# e.set()  # True


def light():
    print('红灯亮...')
    time.sleep(5)
    # 应该发出信号，告诉其他线程准备执行
    e.set()   # 将car中的False变为True
    print('绿灯亮...')


def car(name):
    print('正在等红灯...')
    # 让所有汽车任务进入阻塞态
    e.wait()  # False
    print(f'{name}正在加速飘逸...')


# 让一个light线程控制多个car线程
t = Thread(target=light)
t.start()

for i in range(10):
    t = Thread(target=car, args=(f'汽车{i}号', ))
    t.start()
```