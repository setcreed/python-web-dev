# 事务

事务是什么

事务(Transaction)是访问并可能更新数据库中各种数据项的一个程序执行单元(unit)。事务通常由高级数据库操纵语言或编程语言书写的用户程序的执行所引起。事务由事务开始(begin
transaction)和事务结束(end transaction)之间执行的全体操作组成。

通俗来讲，事务指一组操作，要么都执行成功，要么都执行失败。

## 基本原理：

Mysql允许将事务统一进行管理（存储引擎INNODB），将用户所做的操作，暂时保存起来，不直接放到数据表（更新），等到用于确认结果之后再进行操作。

## 使用方法

```sql
start transaction;
sql 语句
commit(成功提交)/rollback(回滚，清空操作);
```

例子：

```sql
--模拟银行转账

--先准备数据
create table user(
    id int auto_increment primary key,
    name varchar(32) not null default '',
    salary int not null default 0
)charset utf8;

insert into user(name,salary) values ('cwz',1000),('张三',2000);

--开启事务
start transaction;

--修改数据
update user set salary=1800 where name='张三';
update user set salary=1200 where name='cwz';

--提交
commit;   # 这样整个事务结束，上述修改数据的操作才会生效




--若是不想让操作生效，在修改数据操作后
rollback；  # 此时整个事务结束，上述所有操作不执行
```

## 事务的特性

- 原子性(Atomicity)，原子意为最小的粒子，即不能再分的事务。事务从start transaction起到提交事务（commit或者rollback），要么所有的操作都成功，要么就是所有的操作都失败；
- 一致性(Consistency)：指事务发生前和发生后，数据的总额依然匹配；
- 隔离性(Isolation)：简单点说：某个事务的操作对其他事务是不可见的
- 持久性(Durability)：当事务完成后，其影响应该保留下来，不能撤销，只能通过"补偿性事务"来抵消之前的错误。

# 存储引擎

## 什么是存储引擎

数据库对同样的数据,有着不同的存储方式和管理方式，在mysql中,称为存储引擎。

## 存储引擎的特点和分类

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200201204204.png)

## 存储引擎的选择

- 文章,新闻等安全性要求不高的,选myisam
-  订单,资金,账单,火车票等对安全性要求高的,选用innodb
- 对于临时中转表,可以用memory型 ,速度最快

## InnoDB 和 MyIsam

建表时指定存储引擎

```sql
create table user(
    id int auto_increment primary key,
    name varchar(32) not null default ''
)engine=InnoDB charset utf8;
```

在MySQL5.5以上，默认就是InnoDB

InnoDB和MyIsam两个引擎的区别：

1. InnoDB支持事务，MyIsam不支持事务
2. InnoDB支持行锁，MyIsam支持表锁

# 视图

创建视图：

```sql
create view 视图名 as SQL语句;

--例子：
create view v1 as select * from user;
```

删除视图：

```sql
drop view vi;
```

查看视图

```sql
select * from v1;
```

# 触发器

```sql
场景：
    当我下一个订单的时候， 订单表中需要增加一个记录， 同时库存表中需要减1
    这两个操作是同时发生的，  并且前一个操作出发后一个操作
```

使用方法：

```sql
mysql> delimiter //
mysql> create trigger tri_t22_t33 before insert on t22 for each row
    -> begin
    -> insert into t33 (name) values ('aa');
    -> end //
Query OK, 0 rows affected (0.02 sec)

mysql> delimiter ;

insert into t22 (name) valuess
```

查看：

```sql
show triggers \G

*************************** 1. row ***************************
             Trigger: tri_t22_t33
               Event: INSERT
               Table: t22
           Statement: begin
insert into t33 (name) values ('asd');
end
              Timing: BEFORE
             Created: 2019-11-01 18:59:20.19
            sql_mode: ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
             Definer: cwz@%
character_set_client: gbk
collation_connection: gbk_chinese_ci
  Database Collation: latin1_swedish_ci
```

# 存储过程

像一个SQL函数

创建：

```sql
delimiter //
create procedure p1()
begin
    select * from user where id=2;
end //

delimiter;
```

删除：

```sql
drop procedure p1;
```

# 函数

```sql
--返回字符串长度
select char_length('cwzadas');

--字符串拼接
select concat('hello','world');

--format(X, D)  将数字X 的格式写为'#,###,###.##',以四舍五入的方式保留小数点后 D 位， 并将结果以字符串的形式返回。若  D 为 0, 则返回结果不带有小数点，或不含小数部分。
select format(12312.236,2);


--返回字符串 str 中子字符串的第一个出现位置。
INSTR(str,substr)

--返回字符串str 从开始的len位置的子序列字符。
LEFT(str,len)

--变小写
LOWER(str)

--变大写
UPPER(str)

--返回字符串 str ，其引导空格字符被删除。
LTRIM(str)

--返回字符串 str ，结尾空格字符被删去。
RTRIM(str)

--获取字符串子序列
SUBSTRING(str,pos,len)

--获取子序列索引位置
LOCATE(substr,str,pos)

--返回一个由重复的字符串str 组成的字符串，字符串str的数目等于count 。
--若 count <= 0,则返回一个空字符串。
--若str 或 count 为 NULL，则返回 NULL 。
REPEAT(str,count)


--返回字符串str 以及所有被字符串to_str替代的字符串from_str 
REPLACE(str,from_str,to_str)


--返回字符串 str ，顺序和字符顺序相反。
REVERSE(str)


--从字符串str 开始，返回从后边开始len个字符组成的子序列
RIGHT(str,len)
```