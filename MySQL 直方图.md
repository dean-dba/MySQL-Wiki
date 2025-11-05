# MySQL 直方图

### MySQL 从8.0开始推出了直方图，类似于Oracle直方图功能，它的出现本质上是为了辅助优化器选择更优的执行计划，让查询跑的更快。

#### 直方图的基本概念
- **桶（Bucket）数量：0-1024个之间，默认值：100个**  
- **直方图有两种类型：singleton直方图（等宽直方图）、equi-height直方图（等高直方图）；触发规则：在桶不变的情况下，根据列中值的数量自动选择使用那种直方图类型**  
- **8.4 版本开始已支持直方图自动更新**  
- **直方图是针对列的一种统计方式**
-  **支持分区表，但不支持视图（view）**
-  **不支持spatial data、json列类型**
- **主要应用场景是针对没有索引的查询优化和有二级索引但数据分布不均匀导致执行计划不准确的场景**
- **当修改删除表、删除列、修改列属性时，直方图会自动删除**  




创建直方图  
- **在不指定桶（Bucket）数量的情况下，默认会创建100个桶**  
- **在不指定更新方式的情况下需要手动更新** 
``` 
analyze table t update histogram on c1;
```

查看直方图  
```
select * from information_schema.column_statistics where table_name='t';
```

删除直方图  
```
analyze table t drop histogram on c1;
```

直方图相关系统参数，默认值：20000000B（约等于20M） 
- **参数影响直方图的sampling-rate**
```
show variables like '%histogram_generation_max_mem_size%';
```

#### analyze table t 跟 update histogram on 的区别是什么  
- **analyze table针对的是表和索引，而update histogram on针对的是列，维度不同**
- **analyze table的结果是随机采样估算值，受持久化采样参数：innodb_stats_persistent_sample_pages，默认值：20页**
- **analyze table t自动更新与update histogram on自动更新触发时机：当表中10%行发生变化时，会触发自动更新**
- **analyze table t自动更新受参数：innodb_stats_persistent_sample_pages影响，触发自动更新**

##### 特殊说明
- **在MGR集群下，在主节点创建直方图后，从节点并没有同步过来，从节点binlog日志已写入，我理解应该是可以同步过来，应该是BUG，需要官方优化**

#### 最后说明，当发现优化器选择的执行计划不是最优时，可以考虑使用直方图优化

#### 至此，MySQL 直方图安装就介绍完成了，请开始你的表演吧！

#### 链接：https://dev.mysql.com/doc/refman/8.4/en/analyze-table.html

