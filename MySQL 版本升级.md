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

##### 大版本升级步骤：

##### 升级前：
1. 查看官方文档升级版块，主要关注升级路径、版本差异
2. 使用mysqlsh工具，运行util.checkForServerUpgrade()，检查是否具备升级条件
3. 准备独立一套升级环境，测试应用兼容性(接口性能测试、功能测试)
4. 使用sysbench做基准测试
5. 使用流量回放，测试SQL兼容性和性能
6. 制定升级方案、回滚方案
7. 准备配置中心数据源脚本
8. 创建反向同步链路

##### 升级中（低峰期升级）：
1. 升级前全量备份
2. 确认同步工具是否正常，包括用户、数据、参数
3. 停止应用入口流量
4. 修改配置中心数据源
5. 观察新版本实例是否正常

##### 升级后：
1. 修改监控报警、SQL审核数据源
2. 稳定后释放旧实例

##### 小版本升级不需要大版本升级那样复杂，采用原地在线升级即可
- **单机架构：可采用将新版本实例作为旧版本的副本来实现在线升级，也可以采用实时工具实现**
- **主从架构：与单机架构同样方式**
- **MGR架构：可采用添加副本节点方式实现在线升级**

##### 注意事项：主从复制架构，支持低版本源向高版本副本同步；MGR架构只能在主版本+次要版本都一致的情况下才可以添加副本

##### 查看系统参数变更明细：select * from performance_schema.variables_info;

##### 本篇文章不是一个升级的具体命令操作手册，它更多的是一种指导性手册，告诉升级人员应该关注哪些方面，以及如何制定升级方案

##### 至此，关于MySQL 版本升级就介绍完了，有兴趣的小伙伴，赶紧看看你的数据库是不是要升级了！

##### 参数链接：https://dev.mysql.com/doc/refman/8.4/en/upgrading.html

&nbsp;

**有兴趣的小伙伴，可加联系方式：Telegram：@dean_code**  
