## 多进程实现

服务端：

```python
from multiprocessing import Process
import socket


def task(conn, addr):
    while True:
        try:
            data = conn.recv(1024)
            if not data:
                break
            print(addr)
            print(data.decode('utf-8'))
            conn.send(data.upper())

        except Exception:
            break


if __name__ == '__main__':
    server = socket.socket()
    server.bind(('127.0.0.1', 9999))
    server.listen(5)
    while True:

        conn, addr = server.accept()
        print(addr)
        p = Process(target=task, args=(conn, addr))
        p.start()
```

客户端

```python
import socket

client = socket.socket()
client.connect(('127.0.0.1', 9999))

while True:
    send_msg = input('客户端：')
    if send_msg == 'q':
        break

    client.send(send_msg.encode('utf-8'))

    data = client.recv(1024)
    print(data.decode('utf-8'))
```

## 多线程实现

服务端：

```python
import socket
from threading import Thread

server = socket.socket()
server.bind(('127.0.0.1', 9999))
server.listen(5)


def task(conn):
    while True:
        data = conn.recv(1024)
        if len(data) == 0:
            continue

        print(data.decode('utf-8'))

        conn.send(data.upper())


if __name__ == '__main__':
    while True:
        conn, addr = server.accept()
        print(addr)

        t = Thread(target=task, args=(conn,))
        t.start()
```

客户端：

```python
import socket

client = socket.socket()
client.connect(('127.0.0.1', 9999))

while True:
    send_msg = input('客户端：')
    if send_msg == 'q':
        break

    client.send(send_msg.encode('utf-8'))

    data = client.recv(1024)
    print(data.decode('utf-8'))
```