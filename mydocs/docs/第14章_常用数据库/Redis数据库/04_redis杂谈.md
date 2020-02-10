![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200122211934.png)

问题：数据库的表越来越大，那么它的速度也会越来越慢？

不一定

数据库操作行为分为两类：

- 读：查
- 写：增、删、改，这些操作一定会随着表越来越大会受影响

如果是简单查询的话，where条件后面跟的是索引，那么查询速度不会太慢

如果一旦并发量比较大，或者是很复杂的查询，那么会受比较大的影响。

问题在磁盘中，只要数据经过磁盘，IO操作就会变慢



为什么redis使用key-value存储数据

- 建两张表，A表B表，B表中的列受A表中某一列的约束，插入数据只能插入一部分数据，会有一个约束。数据库是全量的，如果把数据移到像redis这样的数据库中，这样就很难控制。









redis和memcache都是k v存储数据的



安装`strace` ，`yum install strace` 追踪线程

```bash
strace -ff -o ~/stracedir/ooxx  ./redis-server

# -ff 追踪所有线程      -o 输出到文件
```

回车，启动redis

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200123094514.png)



看进程号

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200123094552.png)





BGSAVE 命令执行之后立即返回 OK ，然后 Redis fork 出一个新子进程，原来的 Redis 进程(父进程)继续处理客户端请求，而子进程则负责将数据保存到磁盘，然后退出。

客户端可以通过 LASTSAVE 命令查看相关信息，判断 BGSAVE 命令是否执行成功。

BGSAVE  是fork一个save的子进程，在执行save过程中，不影响主进程，客户端可以正常链接redis，等子进程fork执行save完成后，通知主进程，子进程关闭。很明显BGSAVE方式比较适合线上的维护操作，两种方式的使用一定要了解清楚在谨慎选择。



再启动redis客户端，执行`BGSAVE`命令

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200123094923.png)



![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200123095002.png)





![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200123095443.png)





业务处理的时候是单线程的





![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200123101559.png)





![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200123102549.png)





epoll

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200124094126.png)





# [Redis中bitmap的妙用](https://www.cnblogs.com/davidwang456/articles/9362203.html)



需求：

用户系统，任意时间 用户的登录情况

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200124122932.png)





需求2：

推出活动，在仓库送礼物

- 真实用户？
- 冷热用户
- 僵尸用户

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200124123649.png) 