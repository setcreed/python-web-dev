# explain查询性能分析

```sql
mysql> explain select * from t1 where id=2141\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: t1
   partitions: NULL
         type: const
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 4
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)
```

各个参数的含义：

- id：这是select的查询序列号

- select_type：是select的类型

- table：显示这一行的数据是关于哪张表的

- type：这列最重要，显示了连接使用了哪种类别,有无使用索引，是使用Explain命令分析性能瓶颈的关键项之一

  ```sql
结果值从好到坏依次是：
  
  system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL
  
  一般来说，得保证查询至少达到range级别，最好能达到ref，否则就可能会出现性能问题。
  ```
  
- possible_keys：列出MySQL能使用哪个索引在该表中找到行

- key：显示MySQL实际使用的索引

- key_len：索引长度

- ref：显示使用哪个列或常数与key一起从表中选择行。

- rows：扫描的长度

- Extra：使用到了索引



