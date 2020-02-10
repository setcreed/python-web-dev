# 操作数据库

增：

```sql
create database 数据库名称 charset utf8；  # 字符集默认时latin1
```

数据库命名规范：

- 可以由字母、数字、下划线、@、#、$
- 区分大小写
- 具有唯一性
- 不能使用关键字如 select create
- 不能单独使用数字
- 最长128位

删：

```sql
drop database 数据库名称；
```

改：

数据库删除了再添加

线上环境下，不能直接删除数据，再删除之前需要进行备份

查：

```sql
show database;   # 查看数据库
use 数据库名称
```

# 创建数据表

```sql
语法：
    create table 表名(
        字段名   列类型  [可选参数],
        字段名   列类型  [可选参数]
        ....
    ) charset=utf8;
 
 如：
 create table t4(
    id int,
    name char(15)
    )charset=utf8;
```

## 列约束

- auto_increment 自增长1
- primary key 主键索引，加快查询速度，列的值不能重复
- not null 标识该字段不能为空
- default 该字段设置默认值

```sql
create table t5(
    id int primary key auto_increment,
    name char(15) not null default '',
)charset=utf8;
```

# 查看数据表结构

```sql
desc t5;

show create table t5;
```

# 列类型（字段类型）

## 整型

| 类型                 | 大小    | 范围（无符号） |
| -------------------- | ------- | -------------- |
| Tinyint              | 1个字节 | （0-255）      |
| Smallint             | 2个字节 | （0-65535）    |
| Mediumint            | 3个字节 |                |
| int（一般直接用int） | 4个字节 |                |
| bigint               | 8个字节 |                |

注：**unsigned 加上代表不能取负数，只适用于整型。 基本语法：在类型之后加上一个 unsigned**

应用场景：根据公司业务的场景，来选取合适的类型

## 浮点型

- float

Float又称之为单精度类型：系统提供4个字节用来存储数据，但是能表示的数据范围比整型大的多，大概是10^38；只能保证大概7个左右的精度（如果数据在7位数以内，那么基本是准确的，但是如果超过7位数，那么就是不准确的）

基本语法

Float：表示不指定小数位的浮点数

Float(M,D)：表示一共存储M个有效数字，其中小数部分占D位

Float(10,2)：整数部分为8位，小数部分为2位

- decimal

Decimal定点数：系统自动根据存储的数据来分配存储空间，每大概9个数就会分配四个字节来进行存储，同时小数和整数部分是分开的。

Decimal(M,D)：M表示总长度，最大值不能超过65，D代表小数部分长度，最长不能超过30。

**比较**

```sql
mysql> create table t6(
    -> id int unsigned auto_increment primary key,
    -> salary decimal(16,10),
    -> num float
    -> )charset=utf8;
    
    

insert into t6 values(0,500023.2312345678, 5000.2374837284783274832);

mysql> select * from t6;
+----+-------------------+---------+
| id | salary            | num     |
+----+-------------------+---------+
|  1 | 500023.2312345678 | 5000.24 |
+----+-------------------+---------+
```

decimal比较精确，适合描述钱

## 字符串

- **char（长度）**： 定长字符：指定长度之后，系统一定会分配指定的空间用于存储数据，char（L），L长度为0到255
- **varchar（长度）**：变长字符，指定长度之后，系统会根据实际存储的数据来计算长度，分配合适的长度（数据没有超出长度），长度理论值位0到65535

**区别**：

Char和varchar数据存储对比（utf8，一个字符都会占用3个字节）

| 存储数据 | Char(2) | Varchar(2) | Char所占字节 | Varchar所占字节 |
| -------- | ------- | ---------- | ------------ | --------------- |
| A        | A       | A          | 2 * 3 = 6    | 1 * 3 + 1 = 4   |
| AB       | AB      | AB         | 2 * 3 = 6    | 2 * 3 + 1 = 7   |

- char 无论插入的字符是多少个，永远固定占规定的长度
- varchar：根据插入的字符的长度来计算所占的字节数，但是有一个字节是用来保存字符串大小的
- char的数据查询效率比varchar高

## 时间日期类型

### Date

日期类型：系统使用三个字节来存储数据，对应的格式为：YYYY-mm-dd，能表示的范围是从1000-01-01 到9999-12-12，初始值为0000-00-00

### Time

时间类型：能够表示某个指定的时间，但是系统同样是提供3个字节来存储，对应的格式为：HH:ii:ss，但是mysql中的time类型能够表示时间范围要大的多，能表示从-838:59:59~838:59:59，在mysql中具体的用处是用来描述时间段。

### Datetime

日期时间类型：就是将前面的date和time合并起来，表示的时间，使用8个字节存储数据，格式为YYYY-mm-dd HH:ii:ss，能表示的区间1000-01-01 00:00:00 到9999-12-12 23:59:59，其可以为0值：0000-00-00 00:00:00

### Timestamp

时间戳类型：mysql中的时间戳只是表示从格林威治时间开始，但是其格式依然是：YYYY-mm-dd HH:ii:ss

### Year

年类型：占用一个字节来保存，能表示1900~2155年，但是year有两种数据插入方式：0~99和四位数的具体年

```sql
mysql> create table t7(
    -> d date,
    -> t time,
    -> dt datetime
    -> );
    
mysql> insert into t7 values(now(),now(),now());

mysql> select * from t7;
+------------+----------+---------------------+
| d          | t        | dt                  |
+------------+----------+---------------------+
| 2019-10-29 | 15:37:02 | 2019-10-29 15:37:02 |
+------------+----------+---------------------+
1 row in set (0.00 sec)
```

## 枚举enum

就是列举出所有的选项

```sql
mysql> create table t8(
    -> id int unsigned auto_increment primary key,
    -> name char(15) not null,
    -> age int,              ,
    -> gender enum('男','女')
    -> )charset=utf8;
    
    
insert into t8 values(0,'老张',36,1);

mysql> select * from t8;
+----+------+------+--------+
| id | name | age  | gender |
+----+------+------+--------+
|  1 | 老张 |   36 | 男     |
+----+------+------+--------+
```

# 修改表名

```sql
alter table t8 rename info;

rename table t7 to t777;
```

# 增加字段

语法： `alter table 表名 add 字段名 列类型 [可选参数]`

```sql
alter table t6 add name char(25) not null default '';
```

默认是添加在最后一列的

```sql
--添加在第一列
alter table t6 add name char(25) not null default '' first;
--添加在指定列
alter table t6 add name char(25) not null default '' after id;
```

# 删除字段

语法：`alter table 表名 drop 字段名`

```sql
alter table t9 drop name;
```

# 修改字段

修改列类型

语法：`alter table 表名 modify 字段名 列类型 [约束条件]`

```sql
alter table t9 modify name1 char(20);
```

修改字段名:

语法：`alter table 表名 change 旧字段名 新字段名 新的列类型 [约束条件]`

```sql
alter table t6 change sex gender enum('male', 'female')
```

# 删除表

```sql
drop table 表名;
```

# 复制表结构

```sql
create table t99 like t7;
```