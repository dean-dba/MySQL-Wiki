# MySQL Percona XtraBackup工具

#### Percona XtraBackup是一款由Percona基于C语言开发的一个开源物理热备工具，支持MySQL、Percona Server for MySQL、Percona XtraDB数据库，支持InnoDB、MyISAM、XtraDB、MyRocks存储引擎

#### Percona XtraBackup版本说明：(Percona XtraBackup 8.4对应MySQL、Percona Server for MySQL、Percona XtraDB的8.4)、(Percona XtraBackup 8.0对应MySQL、Percona Server for MySQL、Percona XtraDB的8.0)、(Percona XtraBackup 2.4对应MySQL、Percona Server for MySQL、Percona XtraDB的2.4)

#### Percona XtraBackup特点：
- **非阻塞原实例：基于8.0版本LOCK INSTANCE FOR BACKUP，不阻塞DML**
- **并行复制：可以并发复制物理文件，受参数"--parallel"控制**
- **流式备份：可以使用流式备份至远程主机、存储，受参数"--stream"控制，流式格式：xbstream**
- **压缩：支持压缩，减少磁盘空间，受参数"--compress"控制，压缩格式：zstd(默认)、lz4**
- **加密：支持加密，可以保障数据的安全性，受参数"--encrypt"控制**
- **限流，支持限流，避免对源实例IO造成影响，受参数"--throttle"控制**

#### Percona XtraBackup备份功能：全量备份、增量备份、库级备份、表级备份、分区备份
#### Percona XtraBackup恢复功能：全量恢复、部分恢复、库级恢复、表级恢复、分区恢复

全量备份
```
xtrabackup --host=ip --port=3306 --user=user --password=password --estimate-memory=ON --compress --compress-threads=4 --stream=xbstream --dump-innodb-buffer-pool --backup 2> backupout.log|ssh user@ip "cat > /opt/backup/backup_full.xbstream"
```

全量恢复
```
xbstream -x --decompress --decompress-threads=4 -C /opt/aaa < backup_full.xbstream
xtrabackup --prepare --parallel=4 --use-free-memory-pct=50 --target-dir=/opt/aaa/
xtrabackup --copy-back --target-dir=/opt/aaa/
```

恢复单张表
```
xbstream -x --decompress --decompress-threads=4 -C /opt/aaa < backup_full.xbstream
xtrabackup --prepare --parallel=4 --use-free-memory-pct=50 --target-dir=/opt/aaa/
create table aaa (   id int(11) unsigned not null auto_increment,    accountid mediumint(8) not null default '0' ,   addtime int(11) unsigned not null,   primary key (`id`) )  comment='操作记录';
alter table aaa discard tablespace;
复制表文件*.idb
alter table aaa import tablespace;
```

#### 至此关于Percona XtraBackup备份恢复就介绍完成了，有需要的小伙伴可以赶紧用起来了！

#### 参数链接：https://docs.percona.com/percona-xtrabackup/8.4/index.html

&nbsp;

**有兴趣的小伙伴，可加联系方式：Telegram：@dean_code**  

