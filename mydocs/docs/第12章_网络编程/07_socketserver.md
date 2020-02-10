# socketserver

python内置模块，可以简化socket套接字服务端的代码，必须要创建一个类

```python
# server.py
import socketserver

class MyTCPServer(socketserver.BaseRequestHandler):
    # 必须要重写父类的handler
    def handle(self):
        print(self.client_address)

        while True:
            try:
                data = self.request.recv(1024).decode('utf-8')
                send_msg = data.upper()
                self.request.send(send_msg.encode('utf-8'))

            except Exception:
                break


if __name__ == '__main__':
    server = socketserver.ThreadingTCPServer (
        ('127.0.0.1', 8888), MyTCPServer
    )
    server.serve_forever()
    
    
    
    
# client.py
import socket

client = socket.socket()
client.connect(
    ('127.0.0.1', 8888)
)

while True:
    send_msg = input('客户端：')
    if send_msg == 'q':
        break
    client.send(send_msg.encode('utf-8'))

    data = client.recv(1024).decode('utf-8')
    print(data)

client.close()
```