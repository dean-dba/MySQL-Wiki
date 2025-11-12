# MySQL Online DDL

### MySQL Online DDL的目的是最小化对生产环境的影响，使其达到原子化DDL和并发DML的平衡。

#### Online DDL算法类型：
- **instant：** 操作只修改元数据，只在DDL操作开始阶段独占元数据锁，之后允许DML、DQL
- **inplace：** 原地重建表，只在DDL操作开始和结束阶段独占元数据锁，期间允许DML、DQL
- **copy：** 将原表数据copy至新表，开始和结束期间，不允许DML，可以DQL

#### Online DDL锁(lock)类型：
- **none：** 允许DML、DQL
- **shared：** 只允许DQL
- **exclusive：** 不允许DML、DQL  

Online DDL语法
#### 当使用"ALGORITHM=default,LOCK=default"时，MySQL会根据Online DDL操作类型，存储引擎类型、MySQL版本自动匹配对应的ALGORITHM、LOCK，原则是最小化ALGORITHM、LOCK
```
ALTER TABLE tbl_name ADD INDEX name (col_list),ALGORITHM=default,LOCK=default;
```

#### Online DDL声明：
- **MySQL 8.0.29版本之后，添加列时可以使用instant添加列至任意位置，即支持first、after语法**
- **instant 在单表上最大支持64次变更，超出会报错，需要重建表**
- **修改varchar类型时，小于255B内，是inplace，否则是copy，因为小于255B列由1B编码，大于255B需要2B编码**
- **instant、inplace操作期间产生的DML最大数据量受参数：innodb_online_alter_log_max_size控制，默认：128MB**
- **多个同类型的alter table操作，最好合并成一个alter table操作，调用一次api**
- **alter tabler操作期间的DML数据，在应用至新表时才会做约束检查**

 #### 对于inplace、copy的Online DDL操作，可以使用开源工具gh-ost处理，针对gh-ost，我将单独写一篇文章介绍它的用法 

 #### MySQL 8.0版本开始并行创建索引功能，受参数：innodb_ddl_buffer_size(默认：1MB)、innodb_parallel_read_threads(最小4)、innodb_ddl_threads(默认：4) 控制，实际使用中发现，增加innodb_ddl_buffer_size参数值，可以明显加快并行创建索引的速度

 #### 参数链接：https://dev.mysql.com/doc/refman/8.4/en/innodb-online-ddl.html

 
