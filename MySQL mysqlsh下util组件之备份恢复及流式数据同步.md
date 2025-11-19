# MySQL mysqlsh下util组件之备份恢复及流式数据同步

### MySQL 8.0推出了一个工具mysqlsh，它是一款多功能版的mysql客户端，支持SQL、JS、Python三种语言，主要应用于主从、MGR、日常操作、备份恢复、流式数据同步、升级版本检查、备份恢复binlog、性能诊断等场景，可以说它是mysql、mysqlbinlog、mysqldump、mysqlpump、mysql_upgrade、mysqlcheck等这几个客户端的融合版，本章节主要介绍在备份恢复、流式数据同步场景下的应用

**util组件的备份恢复及流式数据同步与克隆插件的最大区别：** util组件的备份恢复及流式数据同步是逻辑备份恢复，克隆插件是物理备份恢复  

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


