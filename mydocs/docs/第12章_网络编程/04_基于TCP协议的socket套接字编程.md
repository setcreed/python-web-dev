# 什么是socket

Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。在设计模式中，Socket其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socket去组织数据，以符合指定的协议。

所以，我们无需深入理解tcp/udp协议，socket已经为我们封装好了，我们只需要遵循socket的规定去编程，写出的程序自然就是遵循tcp/udp标准的。

# 基于TCP协议的套接字编程(简单)

## 服务端

```python
import socket

server = socket.socket()
server.bind(
    ('127.0.0.1', 9999)
)
server.listen(5)

conn, addr = server.accept()
print(addr)

data = conn.recv(1024).decode('utf-8')
print(data)

conn.send('来自服务端消息：我不好'.encode('utf-8'))
conn.close()

server.close()
```

## 客户端

```python
import socket

client = socket.socket()
client.connect(
    ('127.0.0.1', 9999)
)

client.send('来自客户端消息：你好'.encode('utf-8'))


data = client.recv(1024).decode('utf-8')
print(data)

client.close()
```



# 基于TCP协议的套接字编程(复杂)

## 服务端

```python
import socket

server = socket.socket()
server.bind(
    ('127.0.0.1', 9999)
)
server.listen(5)

conn, addr = server.accept()
print(addr)

while True:
    # 接收客户端发送来的消息
    data = conn.recv(1024).decode('utf-8')
    print(data)
    if data == 'q':
        break
    send_msg = input('server--->client: ').encode('utf-8')
    conn.send(send_msg)

conn.close()
server.close()
```



## 客户端

```python
import socket

client = socket.socket()
client.connect(
    ('127.0.0.1', 9999)
)

while True:
    send_msg = input('client---> server：')
    client.send(send_msg.encode('utf-8'))
    if send_msg == 'q':
        break

    # 服务端返回的数据
    data = client.recv(1024).decode('utf-8')
    print(data)

client.close()

```

# 服务端服务多个客户

## 服务端

```python
import socket

server = socket.socket()
server.bind(
    ('127.0.0.1', 8888)
)
server.listen(5)  # 半连接数，等待的用户

while True:
    conn, addr = server.accept()
    print(addr)

    while True:
        try:
            data = conn.recv(1024).decode('utf-8')
            print(data)

            # mac\linux的bug：b''
            if len(data) == 0:
                continue

            if data == 'q':
                break
            send_msg = input('server--->client:').encode('utf-8')
            conn.send(send_msg)
        except Exception as e:
            print(e)
            break

    conn.close()
```



## 客户端

```python
import socket

client = socket.socket()
client.connect(
    ('127.0.0.1', 8888)
)

while True:
    send_msg = input('client--->server：')
    client.send(send_msg.encode('utf-8'))
    if send_msg == 'q':
        break

    data = client.recv(1024).decode('utf-8')
    print(data)

client.close()
```

