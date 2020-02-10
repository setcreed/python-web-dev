# UDP协议

UDP是一种传输协议

1. 不需要建立双向通道
2. 不会粘包
3. 客户端给服务端发送数据，不需要等待服务返回接收成功
4. UDP套接字虽然没有粘包问题，但是不能替代TCP套接字，因为UPD协议有一个缺陷：如果数据发送的途中，数据丢失，则数据就丢失了，而TCP协议则不会有这种缺陷，因此一般UPD套接字用户无关紧要的数据发送，例如qq聊天。

## UDP套接字

```python
# server.py
import socket

server = socket.socket(type=socket.SOCK_DGRAM)

server.bind(
    ('127.0.0.1', 8888)
)

msg, addr = server.recvfrom(1024)
msg1, addr1 = server.recvfrom(1024)
msg2, addr2 = server.recvfrom(1024)
msg3, addr3= server.recvfrom(1024)
print(msg)
print(msg1)
print(msg2)
print(msg3)

# client.py
import socket

client = socket.socket(type=socket.SOCK_DGRAM)

client.sendto(b'hello', ('127.0.0.1', 8888))
client.sendto(b'hello', ('127.0.0.1', 8888))
client.sendto(b'hello', ('127.0.0.1', 8888))
client.sendto(b'hello', ('127.0.0.1', 8888))

'''
b'hello'
b'hello'
b'hello'
b'hello'
'''
```

# 基于UDP实现qq聊天室

```python
# server.py
import socket

server = socket.socket(type=socket.SOCK_DGRAM)
server.bind(('127.0.0.1', 8888))

while True:
    msg, addr = server.recvfrom(1024)
    print(addr)
    print(msg.decode('utf-8'))

    send_msg = input('服务端发送消息：').encode('utf-8')
    server.sendto(send_msg, addr)
    
    
# client.py
import socket

client = socket.socket(type=socket.SOCK_DGRAM)
server_ip_port = ('127.0.0.1', 8888)

while True:
    send_msg = input('客户端1：').encode('utf-8')
    client.sendto(send_msg, server_ip_port)

    msg, addr = client.recvfrom(1024)
    print(msg.decode('utf-8'))
```