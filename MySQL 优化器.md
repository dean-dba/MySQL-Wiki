# MySQL 优化器

### MySQL 优化器就像是SQL的智能大脑，控制着SQL的快慢，同时MySQL 优化器也在不断的迭代，尤其在8.0版本引入了多种优化方式，以及增加了多种优化器提示，可以针对SQL语句进行控制

#### 关于优化器的使用说明
- **全局优化器在生产环境中保持默认即可，即使要动，也要进行充分性测试，避免出现解决了一个问题，却带来了另些一些问题，典型的副作用**
- **生产环境中谨慎使用优化器提示，使用前提：在更新了统计信息，也使用了直方图的情况下，SQL仍然不理想时可用**
- ****

&nbsp;

查看优化器类型
```
SELECT @@optimizer_switch\G;
*************************** 1. row ***************************
@@optimizer_switch: index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=off,skip_scan=on,hash_join=on,subquery_to_derived=off,prefer_ordering_index=on,hypergraph_optimizer=off,derived_condition_pushdown=on,hash_set_operations=on
```

优化器提示语法  
```
/*+ opti_hint */
```

**优化器提示使用范围**：select、insert、update、delete、replace、explain

#### 重要声明
- **关于优化器可参考官网链接：https://dev.mysql.com/doc/refman/8.4/en/switchable-optimizations.html**
- **关于优化器提示可以参考官网链接：https://dev.mysql.com/doc/refman/8.4/en/optimizer-hints.html**
