# MySQL 索引

### MySQL 索引是基于聚集索引表上建立各种索引类型，主要目的是为了加快DQL速度，但会降低DML的速度，在OLTP系统中，读通常大于写，所有这种取舍是值得的

#### MySQL 索引类型
- **主键索引：** 每个表只支持创建一个主键索引，主键索引可以是多列组合，通常不参与业务，与auto_increment配合使用
- **唯一索引：** 每个表可以创建多个唯一索引，与主键索引最大的区别是唯一索引可以为空
- **二级索引：** 表中使用最多的索引类型，支持单列、多列
- **JSON索引：** 8.0开始原生支持在json列上创建索引，底层实现仍然是基于5.7版本的虚拟生成列，只是做了封装，不再需要显示定义
- **HASH索引：** memory存储引擎支持的一种索引类型
- **空间索引：** 基于R-tree算法的一种索引类型
- **不可见索引：** 主要应用场景是调试不同索引间的差异时，可以先将索引置为不可见，不参与优化器的CBO计算模型，便于大表中删除、新增索引带来的时间成本
- **函数索引：** 8.0开始支持的一种索引类型，可以在列上使用函数创建函数索引
- **降序索引：** 8.0开始支持降序索引，原来都是正序排序，即使使用降序语法desc，也会忽略，转为正序
- **前缀索引：** 将列中字段值的前一部分值作业索引的一种索引类型，主要应用场景是大内容字段
- **全文索引：** 类似于ES功能实现的一种索引类型，但建议有此应用场景的还是基于ES建立自己的搜推、算法业务

#### 索引创建过程 （并行索引创建过程同样适用，参考链接：https://github.com/dean-dba/MySQL-Wiki/blob/main/MySQL%20Online%20DDL.md ）
1. 扫描聚集索引表，生成索引数据，并放入排序缓冲区，排序缓冲区满时，会写入临时磁盘文件，排序缓冲区大小受参数："innodb_ddl_buffer_size(默认：1MB)"控制
2. 对临时磁盘文件中的数据进行合并排序
3. 将合并后的排序数据，插入到二级索引中

#### 索引的创建示例

创建主键索引  
```
alter table test add primary key (a);  
```

创建唯一索引  
```
alter table test add unique index idx_a(a);   
```

创建二级索引  
```
alter table test add index idx_a(a); 
```

创建多列索引  
```
alter table test add index idx_a_b(a,b);
```

创建多列正序、降序混合索引  
```
alter table test add index idx_a_b(a asc,b desc);
```

创建函数索引  
```
alter table test add index idx_a((upper(a)));
```

创建前缀索引  
```
alter table test add index idx_a(a(100)); 
```

创建全文索引  
```
alter table test add fulltext index idx_a(a); 
```

创建json单列、单值索引  
```
alter table test add index idx_a_a1((cast(a->>'$.a1' as char(50)) collate utf8mb4_bin)); 
```

创建json单列、多值索引  
```
alter table test add index idx_a_a1( (cast(a->'$.a1' as unsigned array)) );  
```

创建json多列多值索引
```
alter table test add index idx_b_a_a1(modified,(cast(custinfo->'$.zipcode' as unsigned array)) ); 
```

创建不可见索引
```
alter table test alter index idx_a invisible;
```

重命名索引
```
alter table test rename index  idx_a to idx_a_bak;
```

删除索引
```
alter table test drop index idx_a;
```

#### 索引注意事项
- **索引创建后，会自动收集统计信息，受参数"innodb_stats_persistent(默认：on)"控制**
- **索引值的最大大小为3072B，受参数："innodb_default_row_format(默认：dynamic)"控制**
- **索引页合并的阈值是低于50%，受参数："merge_threshold(默认：50%)"控制**
- **索引页保留1/16空间，以供将来增长使用，受参数："innodb_fill_factor(默认：100%)"控制**
- **强制开启表必须有主键索引，受参数："sql_require_primary_key(默认：off)"控制**
- **varchar(10)和char(10)可以使用索引，varchar(10)和char(15)无法使用索引**
- **列数据类型不同无法使用索引**
- **列字符集不同无法使用索引**
- **多值索引DDL算法：copy**
- **order by和group by 列索引必须来自同一索引，否则无法使用索引**
- **多列索引遵循最左前缀原则，但在特殊场景下，也可以使用"skip_scan"特性**
- **一个复合索引只能包含一个多值索引列**
- **JSON类型的cast函数的默认排序规则为utf8mb4_0900_ai_ci，JSON_UNQUOTE()函数的默认排序规则为utf8mb4_bin**
- **前缀索引无法使用覆盖索引**

#### 聚集索引表索引高度与行数计算公式
```
3层高约等于79483712行

16K根节点=(bigint 8字节+子页指针 6字节+额外2字节)+(页头 120字节+页尾 8字节)

根节点数量=(16384-128)/16=1016行

16K页子节点=AVG_ROW_LENGTH 211字节

页子节点数量=(16384-128)/211=77行

表结构：
CREATE TABLE sbtest1 (id bigint NOT NULL AUTO_INCREMENT,k int NOT NULL DEFAULT '0',c char(120) NOT NULL DEFAULT '',pad char(60) NOT NULL DEFAULT '',PRIMARY KEY (id),KEY k_1 (`k`)) ENGINE=InnoDB AUTO_INCREMENT=1000003 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

表大小：
select * from information_schema.tables where table_name='sbtest1';
```

#### 至此，关于MySQL 索引就基本介绍完了，有需要的小伙伴们，赶紧跟着示例使用起来吧！

#### 参考链接：https://dev.mysql.com/doc/refman/8.4/en/create-index.html

&nbsp;

**有兴趣的小伙伴，可加联系方式：Telegram：@dean_code**  
