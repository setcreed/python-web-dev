

# string

字符串类型是redis 中最基本的数据类型,能存储任何形式的字符串,包括二进制数据. 你可以存储用户的邮箱   json化的对象甚至是一张图片.  一个字符串类型键允许存储的数据最大容量是512MB

## 赋值与取值  (set  get)

**SET key value**

将字符串值 `value` 关联到 `key` 。

如果 `key` 已经持有其他值， set 就覆写旧值，无视类型。

```
在Redis中设置值，默认，不存在则创建，存在则修改
参数：
     ex，过期时间（秒）
     px，过期时间（毫秒）
     nx，如果设置为True，则只有name不存在时，当前set操作才执行,值存在，就修改不了，执行没效果
     xx，如果设置为True，则只有name存在时，当前set操作才执行，值存在才能修改，值不存在，不会设置新值
```

## 递增数字  incr

**INCR key**

```
将 key 中储存的数字值增一。

如果 key 不存在，那么 key 的值会先被初始化为 0 ，然后再执行 INCR 操作。

如果值包含错误的类型，或字符串类型的值不能表示为数字，那么返回一个错误。

本操作的值限制在 64 位(bit)有符号数字表示之内
```

![](https://i.loli.net/2019/12/07/ULKG83daZRIxEoi.png)

应用场景:  生成订单号  原子性递增

## 自减  decr

**DECR key**

将 `key` 中储存的数字值减一。

如果 `key` 不存在，那么 `key` 的值会先被初始化为 `0` ，然后再执行 decr 操作。

如果值包含错误的类型，或字符串类型的值不能表示为数字，那么返回一个错误。

```
# 对存在的数字值 key 进行 DECR

redis> SET failure_times 10
OK

redis> DECR failure_times
(integer) 9


# 对不存在的 key 值进行 DECR

redis> EXISTS count
(integer) 0

redis> DECR count
(integer) -1


# 对存在但不是数值的 key 进行 DECR

redis> SET company YOUR_CODE_SUCKS.LLC
OK

redis> DECR company
(error) ERR value is not an integer or out of range
```



## incrby

### 1. 增加

**INCRBY key increment**

将 `key` 所储存的值加上增量 `increment` 。

如果 `key` 不存在，那么 `key` 的值会先被初始化为 `0` ，然后再执行 incrby命令。

如果值包含错误的类型，或字符串类型的值不能表示为数字，那么返回一个错误。

![](https://i.loli.net/2019/12/07/eGs6BKfMg9T31j5.png)

### 2.减少

**DECRBY key decrement**

将 `key` 所储存的值减去减量 `decrement` 。

如果 `key` 不存在，那么 `key` 的值会先被初始化为 `0` ，然后再执行 DECRBY操作。

```
# 对已存在的 key 进行 DECRBY

redis> SET count 100
OK

redis> DECRBY count 20
(integer) 80


# 对不存在的 key 进行DECRBY

redis> EXISTS pages
(integer) 0

redis> DECRBY pages 10
(integer) -10
```

### 3.增加指定浮点数      incrbyfloat

**INCRBYFLOAT key increment**

为 `key` 中所储存的值加上浮点数增量 `increment` 。

如果 `key` 不存在，那么 INCRBYFLOAT会先将 `key` 的值设为 `0` ，再执行加法操作。

如果命令执行成功，那么 `key` 的值会被更新为（执行加法之后的）新值，并且新值会以字符串的形式返回给调用者。

```
无论是 key 的值，还是增量 increment ，都可以使用像 2.0e7 、 3e5 、 90e-2 那样的指数符号(exponential notation)来表示，但是，执行 INCRBYFLOAT 命令之后的值总是以同样的形式储存，也即是，它们总是由一个数字，一个（可选的）小数点和一个任意位的小数部分组成（比如 3.14 、 69.768 ，诸如此类)，小数部分尾随的 0 会被移除，如果有需要的话，还会将浮点数改为整数（比如 3.0 会被保存成 3 ）。

除此之外，无论加法计算所得的浮点数的实际精度有多长， INCRBYFLOAT 的计算结果也最多只能表示小数点的后十七位。
```

## 向尾部追加值  append

**APPEND key value**

如果 `key` 已经存在并且是一个字符串， APPEND命令将 `value` 追加到 `key` 原来的值的末尾。

如果 `key` 不存在， APPEND就简单地将给定 `key` 设为 `value` ，就像执行 `SET key value` 一样。

![](https://i.loli.net/2019/12/07/zWgAOQwEYe4slua.png)

## 获取字符串长度  strlen

**STRLEN key**

返回 `key` 所储存的字符串值的长度。

当 `key` 储存的不是字符串值时，返回一个错误。

```
# 获取字符串的长度

redis> SET mykey "Hello world"
OK

redis> STRLEN mykey
(integer) 11


# 不存在的 key 长度为 0

redis> STRLEN nonexisting
(integer) 0
```



## 同时设置  /  获取多个值

### 1.mset

**MSET key value [key value ...]**

同时设置一个或多个 `key-value` 对。

如果某个给定 `key` 已经存在，那么 MSET会用新值覆盖原来的旧值.

MSET 是一个原子性(atomic)操作，所有给定 `key` 都会在同一时间内被设置，某些给定 `key` 被更新而另一些给定 `key` 没有改变的情况，不可能发生。

### 2.mget

**MGET key [key ...]**

返回所有(一个或多个)给定 `key` 的值。

如果给定的 `key` 里面，有某个 `key` 不存在，那么这个 `key` 返回特殊值 `nil` 

![](https://i.loli.net/2019/12/07/qsDJQM4teg5ylR7.png)

## setrange和getrange

**SETRANGE key offset value**

用 `value` 参数覆写(overwrite)给定 `key` 所储存的字符串值，从偏移量 `offset` 开始。

不存在的 `key` 当作空白字符串处理



**GETRANGE key start end**

返回 `key` 中字符串值的子字符串，字符串的截取范围由 `start` 和 `end` 两个偏移量决定(包括 `start` 和 `end` 在内)。

负数偏移量表示从字符串最后开始计数， `-1` 表示最后一个字符， `-2` 表示倒数第二个，以此类推。

![](https://i.loli.net/2019/12/07/Jg8G7Yz9AKHx53S.png)



## setex

**SETEX key seconds value**

将值 `value` 关联到 `key` ，并将 `key` 的生存时间设为 `seconds` (以秒为单位)。

如果 `key` 已经存在， SETEX命令将覆写旧值。

这个命令类似于以下两个命令：

```
SET key value
EXPIRE key seconds  # 设置生存时间
```

不同之处是， SETEX 是一个原子性(atomic)操作，关联值和设置生存时间两个动作会在同一时间内完成，该命令在 Redis 用作缓存时，非常实用。

## psetex

**PSETEX key milliseconds value**

这个命令和 SETEX 命令相似，但它以毫秒为单位设置 `key` 的生存时间，而不是像SETEX 命令那样，以秒为单位。

