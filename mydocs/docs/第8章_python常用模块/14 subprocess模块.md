# subprocess模块

运行python的时候，我们都是在创建并运行一个进程。像Linux进程那样，一个进程可以fork一个子进程，并让这个子进程exec另外一个程序。在Python中，我们通过标准库中的subprocess包来fork一个子进程，并运行一个外部的程序。
subprocess包中定义有数个创建子进程的函数，这些函数分别以不同的方式创建子进程，所以我们可以根据需要来从中选取一个使用。另外subprocess还提供了一些管理标准流(standard stream)和管道(pipe)的工具，从而在进程间使用文本通信。

```python
import subprocess

while True:
    cmd = input('cmd>>>: ')
    if cmd == 'q':
        break
        
    data = subprocess.Popen(
        cmd,
        shell=True,
        stdout=subprocess.PIPE,  # 返回标准输出结果
        stderr=subprocess.PIPE  # 返回标准错误结果
    )

    res = data.stdout.read() + data.stderr.read()
    print(res.decode('gbk'))
```