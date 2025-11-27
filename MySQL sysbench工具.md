# MySQL sysbench工具

#### sysbench是一个专业的基于LuaJIT开发的高性能多线程基准压测工具，内置多种不同场景的Lua脚本，可以根据不同压测需求，开箱即用，它与TPCC最大的不同，sysbench是专门针对于数据库设计的压测工具，TPCC更偏向于业务侧的全流程压测

#### sysbench支持的压测类型：数据库(MySQL、PostgreSQL)、操作系统(CPU、内存、磁盘、线程、互斥锁)

#### sysbench阶段类型
- **prepare：生成run阶段用的文件、数据**
- **run：运行prepare阶段的结果**
- **cleanup：清空prepare阶段生成的文件、数据**

#### sysbench的5种压测类型
- **uniform(平均分布)：所有数据访问概率相同，应用场景：基准测试、理论性能对比、均匀负载系统**
- **gaussian(高斯分布)：数据访问呈钟形分布，应用场景：中等热点场景、用户行为分析、评分系统**
- **special(特殊分布)：明确的热点数据定义，应用场景：电商热门商品、社交网红内容、缓存测试**
- **pareto(帕累托分布)：符合80/20法则，应用场景：真实业务模拟、容量规划、生产环境测试**

sysbench通用语法
```
sysbench --help
```

sysbench不同场景下的语法
```
sysbench /usr/local/share/sysbench/oltp_read_write.lua help
```

sysbench的prepare阶段
```
sysbench --mysql-host=ip --mysql-port=port --mysql-user=user --mysql-password=password --mysql-db=db --db-driver=mysql --tables=10 --table-size=1000000 --threads=50 --time=120 --report-interval=10 --percentile=99 --histogram=on --rand-type=pareto --rand-pareto-h=0.1 /usr/local/share/sysbench/oltp_read_write.lua prepare
```

sysbench的run阶段
```
sysbench --mysql-host=ip --mysql-port=port --mysql-user=user --mysql-password=password --mysql-db=db --db-driver=mysql --tables=10 --table-size=1000000 --threads=50 --time=120 --report-interval=10 --percentile=99 --histogram=on --rand-type=pareto --rand-pareto-h=0.1 /usr/local/share/sysbench/oltp_read_write.lua run
```

sysbench的cleanup阶段
```
sysbench --mysql-host=ip --mysql-port=port --mysql-user=user --mysql-password=password --mysql-db=db --db-driver=mysql --tables=10 --table-size=1000000 --threads=50 --time=120 --report-interval=10 --percentile=99 --histogram=on --rand-type=pareto --rand-pareto-h=0.1 /usr/local/share/sysbench/oltp_read_write.lua cleanup
```

#### 安装时，新系统可能无法安装，可以选择源码编译安装，因为最新版本1.1.20的发布时间是2020年，2020年以后的新版本系统可能都存在无法直接二进制安装的问题

#### 关于sysbench就介绍完成了，有需要的小伙伴们赶紧用起来吧！

#### 参数链接：https://github.com/akopytov/sysbench/tree/master

&nbsp;

**有兴趣的小伙伴，可加联系方式：Vx：+86 183 1026 3857&nbsp;&nbsp;&nbsp;&nbsp;Telegram：+86 189 1127 2119**  
