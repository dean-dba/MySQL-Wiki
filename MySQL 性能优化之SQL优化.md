# MySQL 性能优化之SQL优化

#### MySQL 性能优化是管理和维护的重中之重，它与业务的SLA直接相关，不管是MySQL、Redis、MongoDB、ES，性能优化包括语句优化、配置优化、架构优化、操作系统优化、硬件优化，这篇文章主要探讨语句优化

#### SELECT优化
- **主键列长度越短越好，因为二级索引中会重复引用它，强烈建议使用自增主键列**
- **组合索引创建时遵循过滤性强的放前面，依次类推，使用时遵循最左匹配原则**
- **表与表、列与列字符集要一致，避免隐式转换，无法使用索引**
- **列与列字段类型要一致，避免隐式转换，无法使用索引**
- **避免使用BLOB、TEXT等非标字段，一是会产生很多内存碎片、二是BLOB会产生磁盘临时表**
- **in值多少受参数"eq_range_index_dive_limit"控制(默认:200)，超出时根据索引统计确定走范围扫描还是全表扫描，范围扫描受参数"range_optimizer_max_mem_size"控制(默认:8M)**
- **默认情况下，表连接使用hash join替代nested_loop，内存使用大小受参数"join_buffer_size"控制(默认:256K)**
- **ICP(索引下推)只适用于二级索引，目的是为了减少回表IO、减少内存使用**
- **MRR(多范围读)基于二级索引将随机回表IO变成了顺序IO，顺序IO内存大小受参数"read_rnd_buffer_size"控制(默认:256K)**
- **内存临时表大小受参数"tmp_table_size"控制(默认:16M)，默认存储引擎"TempTable"，由参数"internal_tmp_mem_storage_engine"控制**
- **where子句与order by子句索引列不同，无法使用索引**
- **order by子句或者group by子句中的索引列不同，会使用临时表**
- **order by子句使用多列组合索引时，列跟索引的组合不连续，无法使用索引**
- **order by、group by子句中多列不是同一个索引，无法使用索引**
- **order by子句无法使用索引时，会使用filesort，受参数"sort_buffer_size"控制(默认:256K)**
- **in、not in、exists、not exists会自动转换成半连接、反连接，受参数"optimizer_switch=semijoin=on"控制**
- **in、not in无法转换成半连接、反连接时，会自动转换成exists、not exists，并使用trigcond处理null值**
- **FROM子句的子查询会自动转换为外部查询，避免使用临时表，受参数"optimizer_switch=derived_merge=on"控制**
- **FROM子句的子查询无法转换为外部查询时，将外层的where条件下推到子查询中提前过滤数据，受参数"optimizer_switch=derived_condition_pushdown=on"控制**
- **mysql索引记录空值，并将多个null值视为同一个值，且null值索引位置在最前面，受参数“innodb_stats_method”控制(默认:nulls_equal)**

  
#### DML优化
- **将多行合并至一行，合并大小受参数"bulk_insert_buffer_size"控制(默认:8M)**
- **LOAD DATA比insert into快20倍**
- **使用mysqlsh工具的util.importTable()、util.loadDump()**
- **避免长事务，直接影响SELECT查询速度，因为受事务隔离性影响，利用MVCC机制需要回表根据事务ID查回滚版本**
- **批量比单行更高效**
- **物理删除使用truncate table**

&nbsp;
- **要善于使用索引解决SQL问题，它是最简单、最直接、最有效的一种解决方案，当索引无法解决问题，可以从更改SQL，修改业务逻辑等方面入手，关于索引，参考链接：<a href="https://github.com/dean-dba/MySQL-Wiki/blob/main/MySQL%20%E7%B4%A2%E5%BC%95.md" target="_blank">MySQL 索引</a>**
- **要熟练使用explain查看执行计划，参考链接：<a href="https://github.com/dean-dba/MySQL-Wiki/blob/main/MySQL%20%E6%89%A7%E8%A1%8C%E8%AE%A1%E5%88%92.md" target="_blank">MySQL 执行计划</a>**
- **要熟练掌握优化器相关的算法来源，参考链接：<a href="https://github.com/dean-dba/MySQL-Wiki/blob/main/MySQL%20%E4%BC%98%E5%8C%96%E5%99%A8.md" target="_blank">MySQL 优化器</a>**


#### 本篇文章不是讲解SQL优化的操作手册，它更多的是一种指导手册，指导在创建表结构时，写SQL时应该注意什么

#### 关于本篇文章会持续追加内容

#### 参考链接：https://dev.mysql.com/doc/refman/8.4/en/optimization.html

&nbsp;

**有兴趣的小伙伴，可加联系方式：Telegram：@dean_code**  
