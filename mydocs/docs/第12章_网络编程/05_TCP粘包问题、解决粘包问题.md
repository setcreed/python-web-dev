# TCP粘包问题

服务端第一次发送的数据，客户端无法精确一次性接收完毕，下一次发送的数据与上一次数据黏在一起了。

1. 无法预测对方需要接收的数据大小长度
2. TCP流式协议，会将多次连续发送数据量小、并且时间间隔短的数据一次性打包发送。

基于TCP的套接字客户端往服务端上传文件，发送时文件内容是按照一段一段的字节流发送的，在接收方看了，根本不知道该文件的字节流从何处开始何处结束。

# 粘包两种情况

1. 服务端需要等缓冲区满才发送出去，造成粘包（发送数据时间间隔很短，数据了很小，会合到一起，产生粘包）

```python
# server.py

import socket

server = socket.socket()
server.bind(
    ('127.0.0.1', 9527)
)
server.listen(5)
conn, addr = server.accept()

data1 = conn.recv(1024)
data2 = conn.recv(1024)
data3 = conn.recv(1024)
print(data1)
print(data2)
print(data3)

# client.py

import socket

client = socket.socket()
client.connect(
    ('127.0.0.1', 9527)
)

client.send(b'hello')
client.send(b'hello')
client.send(b'hello')

client.close()

'''
b'hellohellohello'
b''
b''
'''
```

2. 客户端发送了一段数据，服务端只收了一小部分，服务端下次再收的时候还是从缓冲区拿上次遗留的数据

```python
# 服务端
import socket

server = socket.socket()
server.bind(
    ('127.0.0.1', 9527)
)

server.listen(5)
conn, addr = server.accept()

data1 = conn.recv(2)
data2 = conn.recv(20)

print(data1)
print(data2)

# 客户端
import socket

client = socket.socket()
client.connect(
    ('127.0.0.1', 9527)
)

client.send(b'hello world and shcaondskasd nknasksfn km')

client.close()
'''
b'he'
b'llo world and shcaon
'''
```

# 解决粘包问题

使用struct模块

struct模块是一个可以将很长的数据的长度，压缩成固定的长度的一个标记（数据报头）。

必须先定义报头，发送报头，再发送真实数据

## struct模块的使用

```python
import struct
str1 = '123a56'

# 打包
# i模式会将数据长度压缩成4个bytes
headers = struct.pack('i', len(str1))
print(headers)
print(len(headers))

# 解包
data_len = struct.unpack('i', headers)
print(data_len[0])  # 真实数据长度为len(str1)
```

## 使用struct模块解决粘包

服务端：

```python
import struct
import socket
import subprocess

server = socket.socket()
server.bind(
    ('127.0.0.1', 8888)
)

server.listen(5)
while True:
    conn, addr = server.accept()
    print(addr)
    while True:
        try:
            cmd = conn.recv(1024).decode('utf-8')
            if len(cmd) == 0:
                continue
            if cmd == 'q':
                break
            res = subprocess.Popen(
                cmd,
                shell=True,
                stdout=subprocess.PIPE,
                stderr=subprocess.PIPE)
            data = res.stdout.read() + res.stderr.read()
            # 打包压缩
            headers = struct.pack('i', len(data))
            # 先发送头部
            conn.send(headers)
            # 再发送真实数据
            conn.send(data)

        except Exception:
            break

    conn.close()
```

客户端：

```python
import struct
import socket

client = socket.socket()
client.connect(
    ('127.0.0.1', 8888)
)

while True:
    cmd = input('cmd>>>:')
    client.send(cmd.encode('utf-8'))
    if cmd == 'q':
        break

    # 先获取数据报头
    headers = client.recv(4)
    # 再解包获取真实数据
    data_len = struct.unpack('i', headers)[0]
    # 接收真实数据长度
    data = client.recv(data_len)

    print(data.decode('gbk'))

client.close()
```

## 优化解决粘包问题

把数据真实长度和数据的描述信息一同发送过去

服务端：

```python
import socket
import struct
import json

server = socket.socket()
server.bind(('127.0.0.1', 9999))

server.listen(5)

while True:
    conn, addr = server.accept()
    print(addr)

    while True:
        try:
            # 接收数据报头
            headers = conn.recv(4)

            # 解包获取真实数据长度
            data_len = struct.unpack('i', headers)[0]

            bytes_data = conn.recv(data_len)
            back_dic = json.loads(bytes_data.decode('utf-8'))
            print(back_dic)

        except Exception:
            break

    conn.close()
```

客户端：

```python
import socket
import struct
import json

client = socket.socket()
client.connect(('127.0.0.1', 9999))

while True:
    dic = {
        'file_name': 'shoot on me',
        'file_size': 10000000
    }

    # json序列化
    json_data = json.dumps(dic)
    json_bytes = json_data.encode('utf-8')

    # 压缩
    headers = struct.pack('i', len(json_bytes))

    client.send(headers)
    client.send(json_bytes)
```

# 上传大文件

## 服务端

```python
import socket
import struct
import json

server = socket.socket()
server.bind(('127.0.0.1', 9999))

server.listen(5)


conn, addr = server.accept()
print(addr)

while True:
    try:
        headers = conn.recv(4)
        data_len = struct.unpack('i', headers)[0]
        bytes_data = conn.recv(data_len)

        back_dic = json.loads(bytes_data.decode('utf-8'))
        print(back_dic)

        file_name = back_dic.get('file_name')
        file_size = back_dic.get('file_size')

        init_data = 0
        # 写入文件视频
        with open(file_name, 'wb') as f:
            while init_data < file_size:
                data = conn.recv(1024)
                f.write(data)
                init_data += len(data)
            print(f'{file_name}接收完毕! ')
    except Exception as e:
        print(e)
        break

conn.close()
```

## 客户端

```python
import socket
import struct
import json

client = socket.socket()
client.connect(('127.0.0.1', 9999))


# 1.打开视频文件，获取视频数据
with open(r'D:\pycharm_project\忌日快乐2.mkv', 'rb') as f:
    movie_bytes = f.read()
# 2.为视频组织一个字典
send_dic = {
    'file_name': '忌日快乐2.mkv',
    'file_size': len(movie_bytes)
}
# 3.先打包字典，发送headers，再发字典真实数据
json_data = json.dumps(send_dic)
bytes_data = json_data.encode('utf-8')
headers = struct.pack('i', len(bytes_data))
client.send(headers)
client.send(bytes_data)
# 再发真实视频数据
init_data = 0
num = 1
with open(r'D:\pycharm_project\忌日快乐2.mkv', 'rb') as f:
    while init_data < len(movie_bytes):
        send_data = f.read(1024)
        print(send_data, num)
        num += 1
        client.send(send_data)
        init_data += len(send_data)
```