# MySQL 执行计划

#### 通过MySQL 执行计划，可以很直观的看到SQL的性能，并进行调试和修改，它是了解SQL好坏的最直接工具，也是最常用的方式之一

#### MySQL 执行计划类型

traditional：表格格式，默认格式  
```
explain select col1,col2 from t1 where t1.col1=100;
```

json：json格式，内容详细  
```
explain format=json select col1,col2 from t1 where t1.col1=100;
```

tree：树形格式，按照执行顺序层级展示  
```
explain format=tree select col1,col2 from t1 where t1.col1=100;
```

analyze：采用树形格式，它是执行计划+真实执行，结果会显示执行时间  
```
explain analyze select col1,col2 from t1 where t1.col1=100;
```

explain返回内容  
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key           | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | aaa   | NULL       | ref  | idx_accountid | idx_accountid | 3       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+

##### 也支持通过语法"EXPLAIN FOR CONNECTION ID"语法，显示执行计划
##### 通过"show warnings;"，可以查看重写后的SQL


##### 假设一个二级索引页可以存放多少数据的计算规则如下
innodb_page_size：16K，即16384B
二级索引的一行大小：二级索引列+主键列大小，假设：二级索引列varchar(50)+主键列(bigint)=200+8（因为在utf8mb4字符集下一个字符占4B）

##### 每个索引页存放的行数=16384/208≈79行

##### 假设通过二级索引查找1000行数据，在完全没有缓存的情况下，则磁盘查找次数=1000/79=13次IO（实际IO受磁盘类型、MySQL预读参数等影响）


