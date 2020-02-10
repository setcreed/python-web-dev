# 什么是索引

索引的本质是一个特殊的文件，是存储引擎快速找到记录的一种数据结构。

类比：查字典的过程，通过拼音索引

索引的本质：**通过不断地缩小想要获取数据地范围来筛选出最终想要的结果，同时把随机的事件变成顺序的事件，也就是说，有了这种索引机制，我们可以总是用同一种查找方式来锁定数据。**

# 为什么要使用索引

使用索引就是为了提高查询效率

# 索引的底层原理

使用B+树

# 索引的种类

- 主键索引：加速查找、不为空、不能重复
- 唯一索引：加速查询、不为空的字段不能重复
- 联和唯一索引：unique（name, password）
- 普通索引：加速查找 index(name)
- 联合索引：index(name, password)

# 索引的创建

## 主键索引

新增主键索引

```sql
--创建的时候增加主键索引
create table t1(
    id int auto_increment primary key,
    name varchar(32) not null default ''
)charset utf8;


--表创建完了之后再增加主键索引
 alter table t2 add primary key(id);
 
 
```

删除主键索引

```sql
--删除主键索引
alter table t2 drop primary key;
```

## 唯一索引

新增唯一索引

```sql
--创建表的时候增加唯一索引
mysql> create table t2(
    -> id int auto_increment primary key,
    -> name varchar(32) not null default '',
    -> unique(name)    # 如果不设定唯一键的名字，默认唯一键的名字是作用的字段名
    -> )charset utf8;
    
    
    
--创建表后新增唯一索引键
# create unique index 索引名 on 表名(字段名);
create unique index un_name on t2(name);

--或者
alter table t2 add unique key(name);
--或者
alter table t2 add unique index un_name(name);
```

删除唯一索引键

```sql
alter table t2 drop index name;  # index后面是唯一键的名字
```

## 普通索引

新增

```sql
--创建表的时候新增普通索引
create table t3(
    id int auto_increment primary key,
    name varchar(32) not null default '',
    index u_name(name)   # index后面跟的是索引的名字，可以不写
)charset utf8;

--创建表后创建普通索引
create index 索引名 on 表名(字段名);

create index ix_name on t3(name);


--或者
alter table t3 add index ix_name (name);
```

删除

```sql
alter table t3 drop index u_name;
```

# 索引的优缺点

通过观察`*.ibd`文件可知：

- 索引加快了查询速度
- 但是加了索引之后，会占用大量的磁盘空间

索引不是加的越多越好

# 不会命中索引的情况

- 不能在SQL语句中，进行四则运算，会降低sql的查询效率
- 使用函数 如：`select * from t1 where reverse(name)='abc1213';`
- 类型不一致
  - 如果列是字符串类型，传入条件必须要用引号引起来，如`select * from t1 where email=1241;`
- order by 当排序条件为索引，则select字段必须也是索引字段，否则无法命中
  - `select email from t1 order by email desc;`
  - 如果对主键排序，速度还是很快的
- count(1)或count(列)代替count(*)在mysql中没有差别
- 组合索引最左前缀

# 索引覆盖：

```sql
select id from t1 where id=2132123;
```

当字段id 添加了主键索引，这时候是命中索引的；但查询了字段id，id本身有主键索引，这时候会产生索引覆盖，会比主键索引快。

# 组合索引最左前缀

什么时候会创建联合索引？

**根据公司的业务场景，在最常用的几列上添加索引**

```sql
select * from user where name='abc123' and email='abc123@qq.com'

遇到上述业务情况，错误的做法：
index ix_name(name),
index ix_email(email)


正确的做法：
index ix_name_email(name, email)
```

如果组合索引为：`ix_name_email (name, email)`

那么：

```sql
where name='abc' and email='xxx'   --索引命中
where name='abc'                   --命中索引
where email='abc123@qq.com'        --未命中索引
```