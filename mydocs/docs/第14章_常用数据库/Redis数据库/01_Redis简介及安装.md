# Redis初识

- 完全开源免费
- C语言编写，完全遵守BSD协议
- 是一个高性能的（key/value）分布式内存数据库，基于内存运行
- 支持持久性的NoSql数据库，是当前最热门的NoSql数据库之一

# Redis具有以下3个特点：

- 支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载使用
- 它不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构
- Redis支持数据的备份，即master-slave模式的备份

# Redis典型应用

- 缓存系统
- 计数器
- 消息队列系统
- 排行榜
- 社交网络
- 实时系统



# 安装：

## 自动安装

ubuntu安装很简单：

```bash
sudo apt install redis-server
```

centos7安装：

```bash
yum install redis      # 官方库里的软件包版本太旧了

```

## 编译安装

```bash
# 下载和编译的过程
mkdir redis

cd redis

wget http://download.redis.io/releases/redis-5.0.7.tar.gz

tar xf redis-5.0.7.tar.gz

cd redis-5.0.7  && vim README.md

make

yum install gcc -y

make distclean

make


# 安装过程
make PREFIX=/opt/redis install
vim /etc/profile
# 添加环境变量
...
export REDIS_HOME=/opt/redis
export PATH=$PATH:$REDIS_HOME/bin
...

source /etc/profile

cd utils
./install_server.sh
```



在redis  bin目录下 可执行文件说明

- redis-server       Redis  服务器
- redis-cli              Redis 命令行客户端
- redis-benchmark       Redis性能测试工具
- redis-check-aof          AOF文件修复工具
- redis-check-dump        RDB文件检查工具
- redis-sentinel             Sentinel服务器









