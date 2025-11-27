# MySQL sysbench工具

#### sysbench是一个专业的基于LuaJIT开发的高性能多线程基准压测工具，内置多种不同场景的Lua脚本，可以根据不同压测需求，开箱即用，它与TPCC最大的不同，sysbench是专门针对于数据库设计的压测工具，TPCC更偏向于业务侧的全流程压测

#### sysbench支持的压测类型：数据库(MySQL、PostgreSQL)、操作系统(CPU、内存、磁盘、线程、互斥锁)

#### sysbench阶段类型
- **prepare：生成run阶段用的文件、数据**
- **run：运行prepare阶段的结果**
- **cleanup：清空prepare阶段生成的文件、数据**

sysbench语法
```
[root@mgr-3 sysbench]# sysbench /usr/local/share/sysbench/oltp_read_write.lua help
sysbench 1.0.20 (using bundled LuaJIT 2.1.0-beta2)

oltp_read_write.lua options:
  --auto_inc[=on|off]           Use AUTO_INCREMENT column as Primary Key (for MySQL), or its alternatives in other DBMS. When disabled, use client-generated IDs [on]
  --create_secondary[=on|off]   Create a secondary index in addition to the PRIMARY KEY [on]
  --delete_inserts=N            Number of DELETE/INSERT combinations per transaction [1]
  --distinct_ranges=N           Number of SELECT DISTINCT queries per transaction [1]
  --index_updates=N             Number of UPDATE index queries per transaction [1]
  --mysql_storage_engine=STRING Storage engine, if MySQL is used [innodb]
  --non_index_updates=N         Number of UPDATE non-index queries per transaction [1]
  --order_ranges=N              Number of SELECT ORDER BY queries per transaction [1]
  --pgsql_variant=STRING        Use this PostgreSQL variant when running with the PostgreSQL driver. The only currently supported variant is 'redshift'. When enabled, create_secondary is automatically disabled, and delete_inserts is set to 0
  --point_selects=N             Number of point SELECT queries per transaction [10]
  --range_selects[=on|off]      Enable/disable all range SELECT queries [on]
  --range_size=N                Range size for range SELECT queries [100]
  --secondary[=on|off]          Use a secondary index in place of the PRIMARY KEY [off]
  --simple_ranges=N             Number of simple range SELECT queries per transaction [1]
  --skip_trx[=on|off]           Don't start explicit transactions and execute all queries in the AUTOCOMMIT mode [off]
  --sum_ranges=N                Number of SELECT SUM() queries per transaction [1]
  --table_size=N                Number of rows per table [10000]
  --tables=N                    Number of tables [1]
```
