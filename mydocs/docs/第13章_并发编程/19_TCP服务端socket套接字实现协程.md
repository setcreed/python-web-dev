# TCP服务端socket套接字实现协程

服务端：

```python
from gevent import monkey
from gevent import spawn
import socket

monkey.patch_all()
server = socket.socket()
server.bind(('127.0.0.1', 9999))
server.listen(5)

def task(conn):
    while True:
        try:
            data = conn.recv(1024)
            if len(data) == 0:
                break
            print(data.decode('utf-8'))
            send_data = data.upper()
            conn.send(send_data)

        except Exception:
            break

    conn.close()


def server2():
    while True:
        conn, addr = server.accept()
        print(addr)
        spawn(task, conn)


if __name__ == '__main__':
    s = spawn(server2)
    s.join()
```

客户端：

```python
import socket
from threading import Thread, current_thread

def client():
    client = socket.socket()
    client.connect(('127.0.0.1', 9999))

    number = 0
    while True:
        send_data = f'{current_thread().name} {number}'
        client.send(send_data.encode('utf-8'))

        data = client.recv(1024)
        print(data.decode('utf-8'))
        number += 1


for i in range(400):
    t = Thread(target=client)
    t.start()
```