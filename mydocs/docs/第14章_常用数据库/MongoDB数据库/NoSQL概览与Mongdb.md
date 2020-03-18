# NoSQL介绍

## NoSQL是一类数据库的统称

- 每种NoSQL数据库为特定目的而设计
- NoSQL不是也不可能成为SQL数据库的代替者
- 适用于大数据集、高并发、大流量的应用场景
- 并非完全不用SQL语句

## NoSQL的共性

- 存储结构化数据、但不存储数据关系

## 优点：

- 处理巨大数据集
- 按需扩展（横向增加主句节点）
- 硬件透明，TOC低（传统小机）
- 数据模型松散，方便修改，不下线，快速dirty修复

## 缺点：

- 大部分来自于小型开源项目，缺少技术支持，学习曲线陡峭，部署周期长
- 并不总是符合ACID（原子性、一致性、隔离性、持久性）
- 不保证即时正确记录，多主复制无法保证最新数据（适合互联网，不适合金融）
- NoSQL，没有绝对优势
- 使用已知的技术栈，需要时再迁移（过早优化是万恶之源）



Berkeley DB

Cassandra

Memcached/MemcacheDB

Redis

Riak

Document Stores

CouchDB

MongoDB



# 安装配置MongoDB

```bash
sudo apt install mongodb
```



## 登录

```
mongo   直接本机登录

mongo server_ip:port/db
```

# mongodb组织数据的基本形式

MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

# mongodb操作
## 库的管理语句

- 显示所有的库  show dbs
```
> show dbs
admin  (empty)
local  0.078GB
> 
```

- 切换数据库  use 数据库名
```
> use mydb
switched to db mydb
> 
```

- 查看所在库  db
```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
mydb    0.000GB


> show collections
users
> 
```

- 删除库   db.dropDatabase()
```
> db.dropDatabase()
{ "dropped" : "test", "ok" : 1 }
> 
```

MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。


## 集合的管理语句

- 创建集合
```
> db.createCollection('student')
{ "ok" : 1 }
> 
```

- 显示当前集合
```
> show collections
student
system.indexes
> 
```

- 删除集合

```
> db.student.drop()
true
```

## 数据的增删改查操作

- 插入数据
```
> db.student.insert({name:'reese',age:29})
WriteResult({ "nInserted" : 1 })
>
```
插入多条数据
```
> db.student.insert([
... {name:'neo',age:59,sex:'男'},
... {name:'cwz',sex:'男',age:20,addr:'mon'}])
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
> 
```
- 查询数据
```
> db.users.find()
{ "_id" : ObjectId("5db841899ea8d30495ed512b"), "name" : "neo", "uid" : "1001" }
{ "_id" : ObjectId("5db842269ea8d30495ed512c"), "name" : "lisi", "uid" : 1003, "gid" : [ 1001, 1002, 1003 ] }
> 



> db.users.findOne({uid:1003})
{
	"_id" : ObjectId("5db842269ea8d30495ed512c"),
	"name" : "lisi",
	"uid" : 1003,
	"gid" : [
		1001,
		1002,
		1003
	]
}



> db.users.findOne({uid:{$gt:1002}})     # gt  great than  大于
{
	"_id" : ObjectId("5db842269ea8d30495ed512c"),
	"name" : "lisi",
	"uid" : 1003,
	"gid" : [
		1001,
		1002,
		1003
	]
}
> 

# 默认自动生成_id 字段
```
美观查询
```
> db.student.find().pretty()
{
	"_id" : ObjectId("5b6a3edbbb74e74630ea80f9"),
	"name" : "reese",
	"age" : 29
}
{
	"_id" : ObjectId("5b6a4088bb74e74630ea80fa"),
	"name" : "neo",
	"age" : 59,
	"sex" : "男"
}
{
	"_id" : ObjectId("5b6a4088bb74e74630ea80fb"),
	"name" : "cwz",
	"sex" : "男",
	"age" : 20,
	"addr" : "mon"
}
> 
```

- 修改数据

全文档更新，普通更新
```
> db.student.update({name:'cwz'},{xx:'yy'})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> 
```

指定更新，只会更新第一条数据
```
> db.student.update({name:'reese'},{$set:{age:46}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```

全部更新
```
> db.student.update({name:'reese'},{$set:{age:34},{multi:true})

```

- 删除数据
```
> db.student.remove({name:'cwz'})
# 不加参数会删除所有数据


> db.student.remove({name:'cwz'},{justone:1})
# 删除一条数据

```

# python操作mongodb

建立连接
```python
import pymongo

# 建立连接
client = pymongo.MongoClient('127.0.0.1', 27017)

# 获取数据库
db = client['test']

# 获取集合
col = db['student']

# 测试一下是否成功
data = col.find()
print(data)

```

主要方法：
- insert_one
- insert_many
- update_one
- update_many
- delete_one
- delete_many
- find_one
- find


find_one
```python
import pymongo

# 建立连接
client = pymongo.MongoClient('127.0.0.1', 27017)

# 获取数据库
db = client['test']

# 获取集合
col = db['student']

# 测试一下是否成功
data = col.find_one()
print(data)

# 打印结果：
{'_id': ObjectId('5b6a3edbbb74e74630ea80f9'), 'age': 46.0, 'name': 'reese'}

```

