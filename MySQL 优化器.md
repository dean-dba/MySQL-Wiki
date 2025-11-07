# MySQL 优化器

#### MySQL 优化器就像是SQL的智能大脑，控制着SQL的快慢，同时MySQL 优化器也在不断的迭代，尤其在8.0版本引入了多种优化方式，以及增加了多种优化器提示，可以针对SQL语句进行控制

&nbsp;

查看优化器类型
```
SELECT @@optimizer_switch\G;
*************************** 1. row ***************************
@@optimizer_switch: index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=off,skip_scan=on,hash_join=on,subquery_to_derived=off,prefer_ordering_index=on,hypergraph_optimizer=off,derived_condition_pushdown=on,hash_set_operations=on
```

