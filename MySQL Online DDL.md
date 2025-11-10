# MySQL Online DDL

### MySQL Online DDL的目的是最小化对生产环境的影响，使其达到原子化DDL和并发DML的平衡。

#### Online DDL算法类型：
-**instant：** 操作只修改元数据，只在DDL操作开始阶段独占元数据锁，之后允许DML、DQL
-**inplace：** 原地重建表，只在DDL操作开始和结束阶段独占元数据锁，期间允许DML、DQL
-**copy：**
