# 增加数据

语法：`insert into 表名 (列1, 列2) values (值1,'值2');`

```sql
insert into t1(id,name) values(1,'老赵');
```

# 删除数据

语法：`delete from 表名 where 条件`

```sql
--删除id=1的这一列
delete from t1 where id=1;

--删除表中所有数据
delete from t1;
```

另一种删除的方法：`truncate 表名` 没有where条件

```sql
truncate t1;
```

**区别**：

1. delete删除是一行行删除；而且插入数据从上一次主键自增1开始，
2. truncate删除是全选删除，删除速度高于delete；而且插入数据从1开始

# 修改数据

语法：`update 表名 set 列名1=新值1，列名2=新值2 where 条件`

```sql
update t5 set name='小明' where id=3;
```

# 查询数据

```sql
--条件查询
select * from t4 where id=3;
select * from t4 where id>3;
select * from t4 where id!=3;

--between...and... 取值范围是闭区间
select * from t3 where id between 3 and 8;

--去重查询
select distinct gender from t7;

--in
select * from t6 where id in (2,3,11);

--like模糊查询
select * from t7 where name like 'x%';   x开头
select * from t7 where name like '%x';   x结尾
select * from t7 where name like '%x%';  包含x
```