# MySQL 克隆插件

#### MySQL 8.0开始推出了克隆插件功能，主要应用于MGR集群、主从的全量数据同步，当然也可以作为备份工具如mysqldump、Percona Xtrabackup等备份工具的一种替代或补充

#### 克隆类型
- **本地克隆：** 可以将本实例的数据克隆至本地其它目录，即可以当做本实例的备份，也可以在一台操作系统启动多实例
- **远程克隆：** 通过网络将其它实例上的数据克隆至本地，主要应用于MGR集群、主从复制、数据备份

#### 克隆插件限制
- **源和目标都要安装克隆插件**
- **数据库大版本、次要版本必须相同**
- **字符集、排序规则必须相同**
- **参数"innodb_page_size"、"innodb_data_file_path"必须相同**
- **克隆插件只负责克隆INNODB存储引擎数据，不克隆BINLOG、my.cnf等其它存储引擎格式数据**
- **接收方必须有足够的磁盘空间存储克隆数据**
- **只支持基于实例的物理全备份，不支持基于库、表级别备份**
- **克隆插件默认会清空接收方实例的现有数据，若不想删除，需指定单独目录**

配置文件my.cnf配置方法    
```
plugin_load_add="mysql_clone.so"
```

手动加载方法  
```
INSTALL PLUGIN clone SONAME 'mysql_clone.so';
```

本地克隆语法  
```
CLONE LOCAL DATA DIRECTORY = '/mnt/mysql_data/data';
```

远程克隆语法，注意：当设置为克隆至本地目录后，克隆完成以后，会自动重启MySQL实例  
```
GRANT all on *.* to 'root'@'%';
SET GLOBAL clone_valid_donor_list = 'example.donor.host.com:3306';
CLONE INSTANCE FROM 'donor_clone_user'@'example.donor.host.com':3306 IDENTIFIED BY 'password';
```

远程克隆至其它目录语法  
```
GRANT all on *.* to 'root'@'%';
SET GLOBAL clone_valid_donor_list = 'example.donor.host.com:3306';
CLONE INSTANCE FROM 'donor_clone_user'@'example.donor.host.com':3306 IDENTIFIED BY 'password' DATA DIRECTORY = '/path/to/clone_dir';
```

#### 克隆相关的系统参数
```
+-------------------------------------------+---------+
| Variable_name                             | Value   |
+-------------------------------------------+---------+
| clone_autotune_concurrency                | ON      |
| clone_block_ddl                           | OFF     |
| clone_buffer_size                         | 4194304 |
| clone_ddl_timeout                         | 300     |
| clone_delay_after_data_drop               | 0       |
| clone_donor_timeout_after_network_failure | 5       |
| clone_enable_compression                  | OFF     |
| clone_max_concurrency                     | 16      |
| clone_max_data_bandwidth                  | 0       |
| clone_max_network_bandwidth               | 0       |
| clone_ssl_ca                              |         |
| clone_ssl_cert                            |         |
| clone_ssl_key                             |         |
| clone_valid_donor_list                    |         |
+-------------------------------------------+---------+
```

在接收方查看克隆状态与进度  
```
SELECT * FROM performance_schema.clone_status;
SELECT * FROM performance_schema.clone_progress;
```

#### 生产环境使用克隆插件建议
- **设置参数"clone_block_ddl=on"，避免在克隆期间源实例有DDL阻塞，以及数据不一致**
- **数据量特别大需调整"clone_max_concurrency"、"clone_max_data_bandwidth"、"clone_max_network_bandwidth"，避免对源实例造成负载压力**

#### 注意：克隆插件支持SSL、压缩数据的克隆

#### 至此，关于克隆插件功能就已经全部讲完了，有需要的小伙伴，可以根据需要掌握起来吧！

#### 参数链接：https://dev.mysql.com/doc/refman/8.4/en/clone-plugin.html
