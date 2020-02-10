# hashlib模块

## hash是什么

hash是一种算法（Python3.版本里使用hashlib模块代替了md5模块和sha模块，主要提供 SHA1、SHA224、SHA256、SHA384、SHA512、MD5 算法），该算法接受传入的内容，经过运算得到一串hash值。

```python
import hashlib

m = hashlib.md5()
m.update(b'sayhello')   # 981fe96ed23ad8b9554cfeea38cd334a
print(m.hexdigest())   # 对于不同的字符而言，永不重复
```

## 撞库破解hash算法

```python
pwd_list = [
    'hash3714',
    'hash1313',
    'hash94139413',
    'hash123456',
    '123456hash',
    'h123ash',
]

hash_pwd = '0562b36c3c5a3925dbe3c4d32a4f2ba2'

for pwd in pwd_list:
    m = hashlib.md5()
    m.update(pwd.encode('utf-8'))
    res = m.hexdigest()
    if res in hash_pwd:
        print(f'获取密码成功：{pwd}')  # 获取密码成功：hash123456
```

# hmac模块

密钥 加盐

```python
import hmac

m = hmac.new(b'haha')
m.update(b'hash123456')
print(m.hexdigest())    # 24bb8daab11e526fc9b8178e51bc2ae7


m = m = hmac.new(b'sadness')
m.update(b'hash123456')
print(m.hexdigest())   # df405ffd019d6d3cd9a190fcab33aca5
```