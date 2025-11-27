# MySQL gh-ost工具

#### gh-ost工具是github基于go语言开发的一种MySQL在线DDL变更工具，它与Percona开发的pt-online-schema-change最大不同是gh-ost不是基于触发器设计的，对于大表的DDL变更，影响几乎很小，是生产环境中必备的一个工具之一

#### gh-ost三种连接模式
- **连接从库，在主库转换：这是一种默认的连接模式，在主库进行增量同步，并在从库利用binlog将增量数据异步同步至影子表**
- **连接主库，在主库转换：它是全量+增量都在主库进行，由参数"-allow-on-master"控制**
- **连接从库，在从库转换：它是全量+增量都在从库进行，适用于测试+验证**

#### gh-ost使用要求
- **单机、主从、MGR的binlog_format必须为ROW**
- **单机、主从、MGR的binlog_row_image必须为FULL**
- **默认使用REPEATABLE_READ隔离级别**
- **表必须有主键或者非空唯一键**

#### gh-ost操作步骤
1. 连接主或从，验证权限
2. 在主库创建状态日志表*_ghc和影子表：*_gho，并在*_gho上执行DDL操作
3. 同时进行全量、增量的异步同步（全量时基于块顺序迁移）
4. 锁定原表，将积压的binlog同步完成
5. 将原表重命名，并进行原子切换

#### gh-ost主要特点
- **全量+增量使用单线程处理**
- **可以在线调整块大小，并实时生效**
- **可以监控从节点延迟和线程数等指标，控制速度**
- **可以控制切换时间**
- **可以实时观察切换进度和状态**
- **可以预模拟执行流程**
- **它是原子切换，没有中间状态**

gh-ost变更语法，预执行，关键参数：-execute
```
gh-ost -host=192.168.1.23 -port=3306 -user="root" -password='mysql' -database="bbt" -table="aaa" -alter="modify COLUMN bbb varchar(800) not null" -assume-rbr -chunk-size=5000 -exact-rowcount -concurrent-rowcount -max-load=Threads_running=10 -critical-load=Threads_running=100 -max-lag-millis=2000 -cut-over=atomic -default-retries=5 -panic-flag-file=/tmp/ghost.panic.flag -initially-drop-socket-file -ok-to-drop-table 
```

gh-ost变更语法，执行
```
gh-ost -host=192.168.1.23 -port=3306 -user="root" -password='mysql' -database="bbt" -table="aaa" -alter="modify COLUMN bbb varchar(800) not null" -assume-rbr -chunk-size=5000 -exact-rowcount -concurrent-rowcount -max-load=Threads_running=10 -critical-load=Threads_running=100 -max-lag-millis=2000 -cut-over=atomic -default-retries=5 -panic-flag-file=/tmp/ghost.panic.flag -initially-drop-socket-file -ok-to-drop-table -execute
```

gh-ost变更请求，控制切换时间
```
gh-ost -host=192.168.1.23 -port=3306 -user="root" -password='mysql' -database="bbt" -table="aaa" -alter="modify COLUMN bbb varchar(800) not null" -assume-rbr -chunk-size=5000 -exact-rowcount -concurrent-rowcount -max-load=Threads_running=10 -critical-load=Threads_running=100 -max-lag-millis=2000 -cut-over=atomic -default-retries=5 -panic-flag-file=/tmp/ghost.panic.flag -initially-drop-socket-file -ok-to-drop-table -execute --postpone-cut-over-flag-file=/tmp/ghost.postpone.flag
rm -rf /tmp/ghost.postpone.flag
```

#### gh-ost最新版本还不支持MySQL 8.4版本，因为8.4版本彻底删除了show slave status语法，由show replica status替代;

#### gh-ost可以作为MySQL在线DDL的一种补充，在涉及到inplace、copy算法的变更可以使用它进行无锁迁移

#### 至此，关于gh-ost工具就介绍完了，有需要的小伙伴们可以开始用起来了！

#### 参考链接：https://github.com/github/gh-ost
