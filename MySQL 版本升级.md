### MySQL 版本升级

##### MySQL 版本升级是一个系统性工程，尤其是大版本升级，由于很多参数、数据格式、用法跟旧版本完全不同，且不兼容，需要一个完整的升级方案和回滚方案

##### 版本升级目的：使用最新功能、最优性能、修改BUG、安全等方面的加强修复

##### 升级类型：小版本升级、大版本升级

##### 升级架构：单机、主从、MGR

##### MySQL版本类型：LTS(Long Term Support) Releases：长期支持版本、Innovation Releases：创新版本、Bugfix Releases：BUG修复版本

##### 升级方式：原地升级、逻辑升级、主从或MGR升级

##### 升级关注事项：
- **库、表大小写，受参数"lower_case_table_names"控制**
- **sql_mode模式，受参数"sql_mode"控制**
- **保留字、关键字**
- **非INNODB存储引擎表**
- **分区表、定时任务、视图、触发器、存储过程**
- **事务隔离级别，受参数"transaction_isolation"控制**
- **group_concat函数最大长度，受参数"group_concat_max_len"控制**
- **php短连接，关注参数"wait_timeout"**
- **字符集关注参数"character_set_server"、"collation_server"**
- **timestamp数据类型，关注参数"explicit_defaults_for_timestamp"**
- **慢查询输出阈值，受参数"long_query_time"控制**
- **binlog是否开启、删除周期，受参数"log_bin"、"expire_logs_days"控制**
- **AES加解密模式，受参数"block_encryption_mode"控制**
- **INNODB检查模式，受参数"innodb_strict_mode"控制**
- **更新应用客户端版本**
- **依赖group by默认排序的SQL，要修改成order by**
- **用户密码插件更新至"caching_sha2_password"**

##### 升级步骤：
###### 升级前：
1. 查看官方文档升级版块，主要关注升级路径、版本差异
2. 使用mysqlsh工具，运行util.checkForServerUpgrade()，检查是否具备升级条件
3. 准备独立一套升级环境，测试应用兼容性(接口性能测试、功能测试)
4. 使用sysbench做基准测试
5. 使用流量回放，测试SQL兼容性和性能
6. 做好回滚方案
7. 准备配置中心数据源脚本
      
7. 升级前全量备份
8. 低峰期升级
9. 观察新版本实例
10. 稳定后释放旧实例
11. 使用同步工具，迁移用户、数据
12. 
13. 


