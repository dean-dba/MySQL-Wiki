# MySQL mysqlsh下util组件之备份恢复及流式数据同步

### MySQL 8.0推出了一个工具mysqlsh，它是一款多功能版的mysql客户端，支持SQL、JS、Python三种语言，主要应用于主从、MGR、日常操作、备份恢复、流式数据同步、升级版本检查、备份恢复binlog、性能诊断、导入json等场景，可以说它是mysql、mysqlbinlog、mysqldump、mysqlpump、mysql_upgrade、mysqlcheck等这几个客户端的融合版，本章节主要介绍在备份恢复、流式数据同步场景下的应用

**util组件的备份恢复及流式数据同步与克隆插件的最大区别：** util组件的备份恢复及流式数据同步是逻辑备份恢复，底层基于"LOAD DATA LOCAL INFILE"，克隆插件是物理备份恢复  

dump实例语法、示例
```
util.dumpInstance(outputUrl[, options]) 
util.dumpInstance("/opt/mysql",{dryRun: true,threads: 2,maxRate: "1M"})
```

dump库语法、示例
```
util.dumpSchemas(schemas, outputUrl[, options])
util.dumpSchemas(["test"],"/opt/test",{dryRun: true,threads: 2,maxRate: "1M"})
```

dump表语法、示例
```
util.dumpTables(schema, tables, outputUrl[, options])
util.dumpTables("bbt", ["aaa"], "/opt/aaa", {dryRun: true,threads: 2,maxRate: "1M"})
```

load实例、库、表语法、示例
```
util.loadDump(url[, options])
SET GLOBAL local_infile = 1;
util.loadDump("/opt/mysql", {dryRun: true,deferTableIndexes: "all",analyzeTables: "on",threads:2})
util.loadDump("/opt/mysql",{dryRun: true,deferTableIndexes: "all",analyzeTables: "on",threads:2,includeSchemas:"test"})
util.loadDump("/opt/mysql",{dryRun: true,deferTableIndexes: "all",analyzeTables: "on",threads:2,includeTables: [ "`test`.`table_1`",  "`test`.`table_2`" ]})
```

##### 关于dump、load用法说明
- **默认并发线程数：4**
- **默认不限制速率**
- **默认使用zstd压缩**
- **默认分块：64M/块**
- **默认使用FTWRL**
- **默认忽略库：information_schema,mysql,performance_schema,sys**
- **默认同步创建索引**
- **默认不分析表**

copy语法、示例，###### 语法上跟dump相似，最大不同是cop采用流式复制，没有中间文件，但不支持断点续传
```
util.copyInstance(connectionData[, options])
util.copySchemas(schemaList, connectionData[, options])
util.copyTables(schemaName, tablesList, connectionData[, options])
util.copyInstance('mysql://user@192.168.1.21:3306', {dryRun: true,threads: 2,maxRate: "1M",deferTableIndexes: "all",analyzeTables: "on"})
```

exportTable、importTable语法示例，###### 支持csv、tsv、json等格式
```
util.exportTable(table, outputUrl[, options])
util.exportTable("aaa", "file:///opt/aaa.csv",{dialect:"csv",fieldsEnclosedBy: "\""})
SET GLOBAL local_infile = 1;
util.importTable ({file_name | file_list}, options)
util.importTable("/opt/aaa.csv", {schema: "test", table: "aaa", dialect: "csv", showProgress: true,threads: 2,maxRate: "1M"})
```

导入json文件语法、示例，使用mysqlx协议，默认端口：33060
```
util.importJSON (path, options)
util.importJson("/opt/aaa.json", {schema: "test",table: "aaa",convertBsonTypes: true,convertBsonOid: true,extractOidTime: "_id"})
```
