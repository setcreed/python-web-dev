系统命令（不属于任何一个数据结构）

## keys 命令

KEYS pattern

查找所有符合给定模式 `pattern` 的 `key` 。支持一定的正则匹配

```
keys *   匹配数据库中所有的key
keys h?llo    匹配一个字符
key h[ae]llo   
```

```
KEYS 的速度非常快，但在一个大的数据库中使用它仍然可能造成性能问题，如果你需要从一个数据集中查找特定的 key ，你最好还是用 Redis 的集合结构(set)来代替。
```

时间复杂度：O(N)， `N` 为数据库中 `key` 的数量

返回值：符合给定模式的 `key` 列表。

![](https://setcreed-1258791144.cos.ap-shanghai.myqcloud.com/picgo-img/20191205223737.png)

## exists命令

`exists key`

检查给定 `key` 是否存在

时间复杂度为：O(1)

返回值：若 `key` 存在，返回 `1` ，否则返回 `0` 。

![](https://setcreed-1258791144.cos.ap-shanghai.myqcloud.com/picgo-img/20191205224239.png)

## del 删除

del key1 key2

删除给定的一个或多个 `key` ， 不存在的key会被忽略。

![](https://setcreed-1258791144.cos.ap-shanghai.myqcloud.com/picgo-img/20191205224534.png)

## type命令

type key

返回 `key` 所储存的值的类型。

```
返回值的类型：
none (key不存在)
string (字符串)
list (列表)
set (集合)
zset (有序集)
hash (哈希表)
```



![](https://setcreed-1258791144.cos.ap-shanghai.myqcloud.com/picgo-img/20191205225736.png)



## randomkey

从当前数据库中随机返回(不删除)一个 `key` 。

```
当数据库不为空时，返回一个 key 。
当数据库为空时，返回 nil 。
```

![](https://setcreed-1258791144.cos.ap-shanghai.myqcloud.com/picgo-img/20191205230408.png)



## expire 命令

EXPIRE key seconds    为给定 `key` 设置生存时间，以秒为单位的， 当 `key` 过期时(生存时间为 `0` )，它会被自动删除。

![](https://setcreed-1258791144.cos.ap-shanghai.myqcloud.com/picgo-img/20191205230635.png)



## pexpire

PEXPIRE key milliseconds

这个命令和`expire`命令的作用类似，但是它以毫秒为单位设置 `key` 的生存时间，而不像  `expire`命令那样，以秒为单位。

## pexpireat

PEXPIREAT key milliseconds-timestamp

这个命令和`expire`命令类似，但它以毫秒为单位设置 `key` 的过期 unix 时间戳，而不是像 `expire` 那样，以秒为单位。

## TTL

`ttl key`, 以秒为单位，返回给定 `key` 的剩余生存时间(TTL, time to live)。

```
当 key 不存在时，返回 -2 。
当 key 存在但没有设置剩余生存时间时，返回 -1 。
否则，以秒为单位，返回 key 的剩余生存时间。
```

## PTTL

与TTL类似，唯一不同的就是以毫秒为单位



## sort

最简单的 sort 使用方法是 `SORT key` 和 `SORT key DESC` ：

- `SORT key` 返回键值从小到大排序的结果。
- `SORT key DESC` 返回键值从大到小排序的结果。

