# 数据库是什么

数据库：存储数据的仓库

# 为什么使用数据库

之前用excel来进行管理数据，有诸多问题：

- 电子表格只能处理有限的数据列和数据行，对于数百万玩、数千万等巨大的数据列很难有效地处理
- 电子表格无法提供安全、方便地权限管理和控制手段
- 电子表格很难实现多个数据之间地关联
- 电子表格很难实现并发控制、增量维护等管理方式

这些问题，数据库都能解决，数据库是一种有效地管理大量的、安全的、并发的、关联的、一致的数据管理工具。

# 数据库的分类

## 关系型数据库

关系数据库，是建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法来处理数据库中的数据。

关系模型由关系数据结构、关系操作集合、关系完整性约束三部分组成。

也就是说对于每一列的数据类型会有约束。

典型的关系型数据库：

- MySQL、 MariaDB
- Oracle 甲骨文公司，收费昂贵
- SQL Server 微软出品
- SQLite 小型的文件数据库

## 非关系型数据库

常见的NoSQL一般分为四大类：

1、列存储数据库。如Cassandra, HBase, Riak等，这部分数据库通常是用来应对分布式存储的海量数据。键仍然存在，但是它们的特点是指向了多个列。这些列是由列家族来安排的。

2、key-value 键值存储， 内存数据库。如Redis、Memcache

3、文档型数据库。如MongoDB，用来存储比较灵活的  json格式

4、Elasticsearch数据库，用来全文搜索

# MySQL的架构

类似于socket的客户端和服务端

流程：

1. mysql服务端先启动，监听在一个特定的端口，默认是3306
2. mysql客户端连接服务端
3. mysql客户端就可以发出相关的操作命令，去操作服务端储存的数据

# MySQL的安装（Windows）

1. 官网下载之后5.7版本的
2. 解压到指定目录 如`C:\mysql-5.7.28-winx64`
3. 添加环境变量 ，在系统环境变量Path添加`C:\mysql-5.7.28-winx64\bin`
4. 初始化 `mysqld --initialize-insecure` # 创建data目录，用来存储数据库
5. 启动MySQL服务
   - 可以在cmd（win10需要管理员权限）中运行`mysqld --install` 安装服务
   - 或者在win10服务中启动MySQL
6. 启动MySQL客户端并连接MySQL服务
   - `mysql -u root -p`
7. 修改mysql密码
   - `mysqladmin -uroot -p '原密码' password '要修改的密码'`

常用的参数：

```
-u   user 用户名
-p   password 密码
-h   host主机名或ip    mysql -h 192.168.1.1 -uroot -p
-P   端口 默认是3306   mysql -h 192.168.1.1 -P 3306 -uroot -p
```

忘记密码：

```
先关闭mysqld服务   net stop mysql
在cmd中执行：  mysqld --skip-grant-tables   
再开一个cmd执行： mysql -uroot -p   此时不需要输入密码
执行sql语句： update mysql.user set authentication_string=password('') where user='root';

重启mysql服务
```

# 数据库服务器、数据管理系统、数据库、表、记录的关系

- 数据库服务器：运行数据库管理软件
- 数据库管理软件：管理数据库，如MySQL
- 数据库：用来组织表，相当于win系统中文件夹的概念
- 表：即文件，用来存放多条记录