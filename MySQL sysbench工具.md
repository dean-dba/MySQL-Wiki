# MySQL sysbench工具

#### sysbench是一个专业的基于LuaJIT开发的高性能多线程基准压测工具，内置多种不同场景的Lua脚本，可以根据不同压测需求，开箱即用，它与TPCC最大的不同，sysbench是专门针对于数据库设计的压测工具，TPCC更偏向于业务侧的全流程压测

#### sysbench支持的压测类型：数据库(MySQL、PostgreSQL)、操作系统(CPU、内存、磁盘、线程、互斥锁)

#### sysbench阶段类型
- **prepare：生成run阶段用的文件、数据**
- **run：运行prepare阶段的结果**
- **cleanup：清空prepare阶段生成的文件、数据**

sysbench通用语法
```
sysbench --help
```

sysbench不同场景下的语法
```
sysbench /usr/local/share/sysbench/oltp_read_write.lua help
```

sysbench prepare
```
sysbench --mysql-host=ip --mysql-port=port --mysql-user=user --mysql-password=password --mysql-db=db --db-driver=mysql --tables=10 --table-size=1000000 --threads=50 --time=120 --report-interval=10 --percentile=99 --histogram=on --rand-type=pareto --rand-pareto-h=0.1 /usr/local/share/sysbench/oltp_read_write.lua prepare
```

sysbench run
```
sysbench --mysql-host=ip --mysql-port=port --mysql-user=user --mysql-password=password --mysql-db=db --db-driver=mysql --tables=10 --table-size=1000000 --threads=50 --time=120 --report-interval=10 --percentile=99 --histogram=on --rand-type=pareto --rand-pareto-h=0.1 /usr/local/share/sysbench/oltp_read_write.lua run
```

sysbench cleanup
```
sysbench --mysql-host=ip --mysql-port=port --mysql-user=user --mysql-password=password --mysql-db=db --db-driver=mysql --tables=10 --table-size=1000000 --threads=50 --time=120 --report-interval=10 --percentile=99 --histogram=on --rand-type=pareto --rand-pareto-h=0.1 /usr/local/share/sysbench/oltp_read_write.lua cleanup
```


