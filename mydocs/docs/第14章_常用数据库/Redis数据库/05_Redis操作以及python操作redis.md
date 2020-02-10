# Redis简介

Redis以键值对的形式储存数据

Redis支持的数据类型有：string、list、set、zset(sorted set)、hash


Redis特点：
- 响应速度快，数据量小
- Redis以内存作为数据存储介质，所以读写数据的效率极高

## 安装

下载地址：

- [windows版本](https://github.com/MicrosoftArchive/redis/releases)
- [其他版本](<https://redis.io/>)

# 与其他数据库的比较

## Redis VS MySQL

- redis: 内存数据库(读写快)、非关系型(操作数据方便、数据固定)
- mysql: 硬盘数据库(数据持久化)、关系型(操作数据间关系、可以不同组合)

大量访问的临时数据，才有redis数据库更优

## Redis VS Memcache

- redis: 操作字符串、列表、字典、无序集合、有序集合 ，支持数据持久化(数据丢失可以找回(默认持久化，主动持久化save)、可以将数据同步给mysql) ， 高并发支持
- memcache: 操作字符串 ， 不支持数据持久化 ， 并发量小

# Redis操作

## 启动服务

```python
前提：前往一个方便管理redis持久化文件的路径再启动服务：dump.rdb
1）前台启动服务
>: redis-server
2）后台启动服务
>: redis-server --service-start
3）配置文件启动服务
>: redis-server 配置文件的绝对路径
>: redis-server --service-start 配置文件的绝对路径
eg>: redis-server --service-start D:/redis/redis.conf
```

## 密码管理

- 提倡在配置文件中配置，采用配置文件启动
  requirepass 密码

- 当服务启动后，并且连入数据库，可以再改当前服务的密码(服务重启，密码重置)
  config set requirepass 新密码

- 连入数据库，查看当前服务密码密码
  config get requirepass

## 连接数据库

```python
1）默认连接：-h默认127.0.0.1，-p(端口号)默认6379，-n(数据库编号)默认0，-a(密码)默认无
>: redis-cli

2）完整连接：
>: redis-cli -h ip地址 -p 端口号 -n 数据库编号 -a 密码

3）先连接，后输入密码
>: redis-cli -h ip地址 -p 端口号 -n 数据库编号
>: auth 密码
```

## 关闭服务

- 在没有连接进数据库时执行  `redis-cli shutdown`
- 连接进数据库后执行 `shutdown`



## 数据持久化

- 配置文件的 默认配置
  - save 900 1           超过900秒有1个键值对操作，会自动调用save完成数据持久化
  - save 300 10         超过300秒有10个键值对操作，会自动调用save完成数据持久化
  - save 60 10000     超过60秒有10000个键值对操作，会自动调用save完成数据持久化
- 安全机制
  - 当redis服务不可控宕机，会默认调用一下save完成数据持久化
- 主动持久化
  - save     连入数据库时，主动调用save完成数据持久化
- 注：数据持久化默认保存文件 dump.rdb，保存路径默认为启动redis服务的当前路径

# Redis五种数据类型的操作


## string字符串

- 基本操作
```python
# 设置
set key value  (例如: set name reese)

# 获取value值
get key (例如: get name)

# key是唯一的，不能用同一个key，否则会覆盖


# 设置多个值
mset k1 v1 k2 v2 ...

# 获取多个值
mget k1 k2 ...

# 给key设置过期时间
setex key exp value

# 将 key 所储存的值加上增量 increment 。
incrby key increment
```

- 查看过期时间

```python
127.0.0.1:6379> ttl name
(integer) -1

# -1代表永久， -2代表不存在
```

- 设置过期时间

```python
# 给已存在的key设置过期时间
expire key seconds   (例子：expire name 10)

# 设置key的同时，设置过期时间
set key value ex seconds  (例如: set name reese ex 20)
或者
setex key seconds value   (例如: setex name 20 cwz)
```

- 追加

```python
# 给已有的value值，添加新的值

append key value

```

- 设置/获取多个

```python
# 设置多个string
mset key value key value ...

例如：
mset user cwz password 123


# 获取多个
mget key key key ...

例子：
mget user password
```

- key操作

```python
# 查看所有的key值
keys *

# 删除
del key

# 查看key是否存在，存在返回1，不存在返回0
exists key

# 查看key类型
type key

```

- 运算

```python
set num 1   # 自动识别，字符串里面的整数

incr num    # 加1

decr num    # 减1

incrby num 50  # 增加多个

descby num 50  # 减少多个
```


## list列表

栈：先进后出  

队列：先进先出

- 设置

```python
# 左添加   栈 先进后出
127.0.0.1:6379> lpush my_list 1 2 3 4 5 6 
(integer) 6

# 右添加   队列 先进先出
rpush my_list 7 8
```

- 查看

```
查看所有：
127.0.0.1:6379> lrange my_list 0 -1
1) "6"
2) "5"
3) "4"
4) "3"
5) "2"
6) "1"
```

- 获得list的元素个数

```
llen my_list
```

- 查看特定索引位置的元素

```
lindex my_list 2
```

- 删除

```python
lpop my_list     # 删除左边第一个

rpop my_list     # 删除右边第一个


lrem my_list 1 5  # 表示从左往右删除1个5

lrem my_list 0 5  # 表示删除所有的5

lrem my_list -2 5  # 表示从右往左删除2个5

```

## hash(字典)

是一个string类型的field和value的映射表，特别适合用于存储对象

hash的key必须是唯一的

Key : (field:value)

- 设置

```
hset key field value

# 例子：
hset users name xxx
```

- 获取

```
hget key field

# 例子：
hget users name
```

- 删除

```
hdel key field

# 例子：
hdel users name
```

- 设置多个

```
hmset user name yyy age 19 sex male
```

- 获取多个

```
hmget user name age sex
```
- 获取全部field value

```
hgetall user
```

- 获取所有的field

```
hkeys user
```

- 获取所有的value

```
hvals user
```

- 获取field个数

```
hlen key
```

- 获取field类型

```
type key
```


## set集合


- 设置

```
sadd my_set 1 2 3 4 5 6
```

- 获取

```
smembers key
```

- 删除

```
# srem指定删除
srem key members

# spop随即删除
spop key
```

- 移动一个集合的值到另一个集合

```
smove oldkey newkey members

例子：
smove my_set1 my_set2 3
```

- 判断集合存在某个值

```
sismember key value
```

- 交集

```
sinter key1 key2 ...


# 把key1 key2的交集合并到newkey
sinterstore newkey key1 key2
```

- 并集

```
sunion key1 key2 ...

# 把key1 key2的并集合并到newkey
sunionstore newkey key1 key2
```

- 差集
```
sdiff key1 key2 ...

# 把key1 key2的差集合并到newkey
sdiffstore newkey key1 key2

```

- 获取集合个数

```
scard key 
```

- 随机返回一个数据

```
srandmember key
```


## sorted set 有序集合

- 设置
```
zadd key score member   (权，权重，顺序)

例子:
127.0.0.1:6379> zadd my_zset1 1 1 2 2 3 3 4 4 5 5 6 6

127.0.0.1:6379> zrange my_zset1 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
```

- 查询获取

```
# zrange 正序

zrange my_zset1 0 -1


# zrevrange 倒序

zrevrange my_zset1 0 -1
```

- 删除

```
zrem my_zset1 2
```

- 索引

```
# zrank 正序
zrank key member

# zrevrank 反序
zrevrank key member

```

- zcard 查看有序集合元素数

```
zcard key
```

- zrangebyscore 返回集合中score在给定区间的元素

```
# zrange my_zset 0 -1 withscores

zrangebyscore my_zset 2 3 withscores
# 返回了score在2~3区间的元素
```

- zcount 返回集合中在定区间的数量
```
zount key min max

例子
zount my_zset 2 3
```

- zscore  查看score(权重)值

```
zscore key member

例子
zscore my_zset 3
```

- zremrangebyrank  删除集合中排名在定区间中的元素
```
# zrange my_zset 0 -1 withscores
zrerangebyrank my_zset 1 3
```

- zremrangebyscore 删除集合中score在给定区间的元素

```
# zrange my_zset 0 -1 withscores
zremrangebyscore my_zset 1 3
```



# python操作Redis

安装 包   `pip install redis`

## 直接使用

```python
import redis

rdb = redis.Redis(
    host='127.0.0.1',
    port=6379,
    db=0,
    password=None,
    decode_responses=True
)
rdb.set('name', 'reese')
print(rdb.get('name'))
```

## 连接池使用

```python
import redis

pool = redis.ConnectionPool(
    host='127.0.0.1',
    port=6379,
    db=0,
    password=None,
    decode_responses=True
)
rdb = redis.Redis(connection_pool=pool)

print(rdb.get('name'))
```

一些操作：

```python
rdb.expire('user_name', 20)  # 添加过期时间

rdb.ttl('user_name') # 在python中不能查看

rdb.mset(a=1, b=2)   # 设置多个

rdb.incr('num',222)  # 后面直接加参数为数量，如果不加参数，默认加1

rdb.lrem('list_1', 3, 0) # 要删的数量在后面，删除的元素在前面

rdb.hmset('user', {'name':'neo', 'age':19}) # key单独写出，后面用字典方式添加
```

