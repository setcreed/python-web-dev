# pymysql模块的安装

```
pip3 install pymysql
```

# python连接数据库

```sql
import pymysql

# 连接参数
conn = pymysql.connect(
    host='localhost',
    port=3306,
    user='cwz',
    password='123',
    database='work',
    charset='utf8'
)

cursor = conn.cursor()   # 执行结果默认以元组形式返回

sql = 'select * from t1'  # 执行的sql语句

cursor.execute(sql)   # 执行sql语句
res = cursor.fetchall()  # 取出所有数据

print(res)

cursor.close()
conn.close()
```

## pymysql的参数

```sql
import pymysql

# 连接参数
conn = pymysql.connect(
    host='localhost',
    port=3306,
    user='cwz',
    password='123',
    database='work',
    charset='utf8'
)

cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)   # 执行结果以列表套字典的方式返回

sql = 'select * from t1'  # 执行的sql语句

cursor.execute(sql)   # 执行sql语句
# res = cursor.fetchall()  # 取出所有数据
# res = cursor.fetchone()  # 取出一条数据，以字典形式返回
res = cursor.fetchmany(2)  # 取出指定条数据 以列表套字典的方式返回

print(res)

cursor.close()
conn.close()
```

# sql注入问题

## 模拟登录

```sql
import pymysql

user = input('请输入用户名：').strip()
pwd = input('请输入密码：').strip()

conn = pymysql.connect(
    host='localhost',
    port=3306,
    user='cwz',
    password='123',
    database='work',
    charset='utf8'
)

cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

sql = f'select * from t1 where name="{user}" and password="{pwd}"'

cursor.execute(sql)
res = cursor.fetchall()
print(res)

cursor.close()
conn.close()

if res:
    print('登录成功')

else:
    print('登录失败')
```

## execute()之sql注入问题

![img](https://images.cnblogs.com/cnblogs_com/setcreed/1579889/o_mysql1.jpg)



![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200201203100.png)



## 解决方法

```sql
import pymysql

user = input('请输入用户名：').strip()
pwd = input('请输入密码：').strip()

conn = pymysql.connect(
    host='localhost',
    port=3306,
    user='cwz',
    password='123',
    database='work',
    charset='utf8'
)

cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

sql = 'select * from t1 where name=%s and password=%s'
cursor.execute(sql, ('aa', '123'))

res = cursor.fetchall()
print(res)

if res:
    print('登录成功')

else:
    print('登录失败')

cursor.close()
conn.close()
```

# 操作MySQL数据库

## 增加数据

```sql
import pymysql

conn = pymysql.connect(
    host='localhost',
    port=3306,
    user='cwz',
    password='123',
    database='work',
    charset='utf8'
)

cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

sql = 'insert into t1 (name,password) values (%s,%s)'

cursor.executemany(sql,(('a2','123'),('a3','123'))) # executemany插入多条数据
print(cursor.lastrowid)   # 获取最后一行id值

conn.commit()  # 一定要有这一步，否则就不能修改

cursor.close()
conn.close()
```

## 修改数据

```sql
import pymysql

conn = pymysql.connect(
    host='localhost',
    port=3306,
    user='cwz',
    password='123',
    database='work',
    charset='utf8'
)

cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

sql = 'update t1 set name=%s where id=%s'

cursor.execute(sql,('qwe123',2))


conn.commit()  # 一定要有这一步，否则就不能修改

cursor.close()
conn.close()
```

## 删除数据

```sql
import pymysql

conn = pymysql.connect(
    host='localhost',
    port=3306,
    user='cwz',
    password='123',
    database='work',
    charset='utf8'
)

cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

sql = 'delete from t1 where id=%s'

cursor.execute(sql,(3,))


conn.commit()  # 一定要有这一步，否则就不能修改

cursor.close()
conn.close()
```