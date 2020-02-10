# PostgreSQL简介

PostgreSQL标榜自己是世界上最先进的开源数据库。PostgreSQL的一些粉丝说它能与Oracle相媲美，而且没有那么昂贵的价格和傲慢的客服。最初是1985年在加利福尼亚大学伯克利分校开发的，作为Ingres数据库的后继。PostgreSQL是完全由社区驱动的开源项目。它提供了单个完整功能的版本，而不像MySQL那样提供了多个不同的社区版、商业版与企业版。PostgreSQL基于自由的BSD/MIT许可，组织可以使用、复制、修改和重新分发代码，只需要提供一个版权声明即可。

## PostgreSQL与MySQL对比

PostgreSQL的优势

-  PostgreSQL完全免费，而且是BSD协议，如果你把PostgreSQL改一改，然后再拿去卖钱，也没有人管你，这一点很重要，这表明了PostgreSQL数据库不会被其它公司控制。oracle数据库不用说了，是商业数据库，不开放。而MySQL数据库虽然是开源的，但现在随着SUN被oracle公司收购，现在基本上被oracle公司控制，其实在SUN被收购之前，MySQL中最重要的InnoDB引擎也是被oracle公司控制的，而在MySQL中很多重要的数据都是放在InnoDB引擎中的，反正我们公司都是这样的。所以如果MySQL的市场范围与oracle数据库的市场范围冲突时，oracle公司必定会牺牲MySQL，这是毫无疑问的。
- 与PostgreSQl配合的开源软件很多，有很多分布式集群软件，如pgpool、pgcluster、slony、plploxy等等，很容易做读写分离、负载均衡、数据水平拆分等方案，而这在MySQL下则比较困难。
-  PostgreSQL在很多方面都比MySQL强，如复杂SQL的执行、存储过程、触发器、索引。同时PostgreSQL是多进程的，而MySQL是线程的，虽然并发不高时，MySQL处理速度快，但当并发高的时候，对于现在多核的单台机器上，MySQL的总体处理性能不如PostgreSQL，原因是MySQL的线程无法充分利用CPU的能力。

# 使用PostgreSQL

官网是：https://www.postgresql.org/

可以看官网找到对应平台的下载安装方法

我使用centos7安装

## 安装

```bash
yum install -y postgresql-server
```

## 数据库初始化

```bash
service postgresql initdb
```

## 启动postgresql服务

```bash
systemctl start postgresql   # 开启服务
systemctl enable postgresql   # 开机自启
```

## 查看postgresql状态

```bash
# postgresql默认 侦听在5432端口
netstat -pantul | grep 5432

ss -pantul | grep 5432

```

## 连接数据库

默认安装会自动创建一个操作系统账号

```bash
cat /etc/passwd

postgres:x:26:26:PostgreSQL Server:/var/lib/pgsql:/bin/bash
```



```bash
# 切换到postgres账号运行 psql


[root@localhost ~]# su - postgres
Last login: Fri Jan 24 17:35:07 CST 2020 on pts/0
-bash-4.2$


-bash-4.2$ psql
psql (9.2.24)
Type "help" for help.

postgres=#
```



```bash
postgres=# \l  # 列出所有数据库
                             List of databases
   Name    |  Owner   | Encoding  | Collate | Ctype |   Access privileges
-----------+----------+-----------+---------+-------+-----------------------
 postgres  | postgres | SQL_ASCII | C       | C     |
 template0 | postgres | SQL_ASCII | C       | C     | =c/postgres          +
           |          |           |         |       | postgres=CTc/postgres
 template1 | postgres | SQL_ASCII | C       | C     | =c/postgres          +
           |          |           |         |       | postgres=CTc/postgres
(3 rows)


postgres=# \q       #退出数据库
-bash-4.2$ 
```

## 修改密码

```sql
# 修改postgres用户的密码，以加密的方式设置密码，默认加密方式是md5
alter user postgres with encypted password '123'; 

\q   # 退出

```

修改了密码，下次登录的时候需要 以md5加密的方式，把你输入密码的密文提交给服务器，服务器会拿着提交的密文去数据库里比对。

所以要修改一下配置文件，postgresql数据库的配置主要是通过修改数据目录下的postgresql.conf文件来实现的。`/var/lib/pgsql/data/`，这是yum安装默认路径

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200124182032.png)

```bash
vim /var/lib/pgsql/data/pg_hba.conf   # 这个配置文件是控制客户端登录，管理操作界面的时候，以谁的身份、以什么方式提交密码等
```

`pg_hba.conf`该文件用于控制访问安全性，管理客户端对于PostgreSQL服务器的访问权限，内容包括：允许哪些用户连接到哪个数据库，允许哪些IP或者哪个网段的IP连接到本服务器，以及指定连接时使用的身份验证模式。

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200124183618.png)



```bash
# 重启服务
systemctl restart postgresql
```



```bash
# 这时登录postgresql就需要密码了

psql -U postgres -W      # -U 以什么身份登录， -W 提示输入密码
```



## 配置允许远程连接

```bash
vim /var/lib/pgsql/data/postgresql.conf   # 网络身份认证配置文件

# 找到监听的选项，修改地址
listen_addresses = '*'   # 所有的地址都能连接

# 在pg_hba.conf文件中添加客户端连接的ip地址
# 添加如下配置，我这里是默认所有的客户端都能访问
host   all    all    0.0.0.0/0     md5

# 保存退出，并重启服务
systemctl restart postgresql
```

注意在centos系统上要开放5432端口

## 创建账号、创建数据库、删库

创建普通账号

```sql
# 进入postgresql，psql -U postgres -W
# 创建账号和密码
create user zhangsan with password '123';

# 创建数据库，基于用户zhangsan
create database mydb owner zhangsan;

# 授权用户
grant all privileges on database mydb to zhangsan;

# 删除库
drop database mydb;
```

## 创建表

```sql
# 使用mydb数据库
\c mydb;

# 创建表
create table users (
	uname varchar(25),
    password varchar(25),
    age int
);
```

## 插入数据

```sql
insert into users values ('lisi', '123', '21');
```

批量插入数据

```sql
copy users from '/home/user/users.txt';
```

## 一些与MySQL不一样的命令

```sql
1、列出所有数据库
mysql: show databases
psql:    \l或\list
2、切换数据库
mysql:  use dbname
psql:     \c dbname
3、列出当前数据库下的所有表
mysql:  show tables
psql:       \d
4、列出指定表的所有字段
mysql:   shwo columns from table_name
psql:    \d table_name
5、查看表的基本情况
mysql:  describe table_name
psql:    \d+ table_name
```



# python操作postgresql

PostgreSQL可以使用`psycopg2`模块与Python集成。

```python
pip3 install psycopg2
```



## 连接到数据库

```python
import psycopg2

conn = psycopg2.connect(
    database="mydb",
    user="postgres",
    password="123",
    host="192.168.32.129",
    port="5432"
)

print("Opened database successfully")
```

## 增加数据

```python
import psycopg2

conn = psycopg2.connect(
    database="mydb",
    user="postgres",
    password="123",
    host="192.168.32.129",
    port="5432"
)

cursor = conn.cursor()
sql = 'insert into users (uname, password, age) values (%s, %s, %s)'
cursor.execute(sql, ('cwz', '123', 18))
conn.commit()
cursor.close()
conn.close()
```

