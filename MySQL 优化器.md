# MySQL 优化器

### MySQL 优化器就像是SQL的智能大脑，控制着SQL的快慢，同时MySQL 优化器也在不断的迭代，尤其在8.0版本引入了多种优化方式，以及增加了多种优化器提示，可以针对SQL语句进行控制

#### 关于优化器的使用说明
- **全局优化器在生产环境中保持默认即可，即使要动，也要进行充分性测试，避免出现解决了一个问题，却带来了另些一些问题，典型的副作用**
- **生产环境中谨慎使用优化器提示，使用前提：在更新了统计信息，也使用了直方图的情况下，SQL仍然不理想时可用**
- **可以通过启用Optimizer Trace查看优化器执行过程**

查看优化器类型
```
SELECT @@optimizer_switch\G;
*************************** 1. row ***************************
@@optimizer_switch: index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=off,skip_scan=on,hash_join=on,subquery_to_derived=off,prefer_ordering_index=on,hypergraph_optimizer=off,derived_condition_pushdown=on,hash_set_operations=on
```

优化器提示使用语法  
```
/*+ opti_hint */
```

优化器使用案例  
```
select /*+ JOIN_ORDER(t1, t2) t1.col_name,t2.col_name from t1,t2 where t1.id=t2.id; 
```

查看优化器执行方式
```
explian select format=json t1.col_name,t2.col_name from t1,t2 where t1.id=t2.id;  
show warnings; 
```

启用Optimizer Trace，默认格式：json
```
SET SESSION optimizer_trace = "enabled=on";
SELECT f1, f2 FROM t1 WHERE f2 > 40;
SELECT * FROM information_schema.optimizer_trace\G;
```

Optimizer Trace相关参数
```
mysql> show variables like '%trace%';
+------------------------------+----------------------------------------------------------------------------+
| Variable_name                | Value                                                                      |
+------------------------------+----------------------------------------------------------------------------+
| optimizer_trace              | enabled=on,one_line=off                                                    |
| optimizer_trace_features     | greedy_search=on,range_optimizer=on,dynamic_range=on,repeated_subselect=on |
| optimizer_trace_limit        | 1                                                                          |
| optimizer_trace_max_mem_size | 1048576                                                                    |
| optimizer_trace_offset       | -1                                                                         |
+------------------------------+----------------------------------------------------------------------------+
```

**优化器成本模型：** 基于成本模型（CBO：Cost-based Optimizer）  

**优化器成本模型数据来源：** mysql.server_cost、mysql.engine_cost

**优化器提示使用范围：** select、insert、update、delete、replace、explain  

**优化器提示分类：** 连接顺序、表级、索引级、子查询、SQL执行时间、资源组、设置变量、语句块命名  

**优化器提示应用范围：** 全局、查询块、表级、索引级  

#### 重要声明
- **关于优化器可参考官网链接：** https://dev.mysql.com/doc/refman/8.4/en/switchable-optimizations.html
- **关于优化器提示可以参考官网链接：** https://dev.mysql.com/doc/refman/8.4/en/optimizer-hints.html
- **关于优化器成本模型可以参考官网链接**：https://dev.mysql.com/doc/refman/8.4/en/cost-model.html

**特别说明：如果用家庭自驾出游来形容优化器成本模型、优化器、优化器提示，可以这么描述：** 优化器成本模型就像是吃喝拉撒的评估手册、优化器就像是吃喝拉撒的最优建议、而优化器提示就像是吃喝拉撒的任意妄为  

**至此，关于MySQL 优化器部分就介绍完了，有需要的小伙们们，可以试着感受下优化器带给你的奇妙之旅吧！**

&nbsp;

**有兴趣的小伙伴，可加联系方式：Telegram：@dean_code**  
