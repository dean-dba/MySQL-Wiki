# MySQL 资源组

#### MySQL 8.0推出了资源组功能，目前支持绑定逻辑CPU的核数，它的主要应用场景是在单实例中为一些耗费资源比较高的任务设置不同的资源优先级的一种精细化控制方式，避免影响业务的正常处理

##### 资源组的基于概念  
- **资源组分为两种：系统资源组、用户资源组；系统资源组优先级：-20到0，用户资源组优先级：0到19**
- **系统资源组和用户资源组默认优先级为0**
- **系统资源组和用户资源组默认无法删除**
- **资源组操作不会写入binlog，也不会复制**

创建资源组  
```
CREATE RESOURCE GROUP batch TYPE=USER VCPU=0 THREAD_PRIORITY=10;
```

查看资源组  
```
select * from information_schema.RESOURCE_GROUPS;
```

修改资源组  
```
ALTER RESOURCE GROUP batch THREAD_PRIORITY = 20;
 ```

资源组应用范围
```
线程级：SET RESOURCE GROUP batch FOR thread_id;
```

 ```
会话级：SET RESOURCE GROUP batch;
 ```

 ```
语句级：INSERT /*+ RESOURCE_GROUP(batch) */ INTO aaa VALUES(1,1,1);
 ```

查看线程使用的资源组
```
select THREAD_ID,NAME,TYPE,PROCESSLIST_ID,PROCESSLIST_STATE,RESOURCE_GROUP from performance_schema.threads;
```

##### 在Linux操作系统中，资源组的使用受CAP_SYS_NICE功能的限制，想在Linux操作系统中使用资源组，需将CAP_SYS_NICE打开，打开方式有两种：  
- **对于使用systemd方式的操作系统，可以在mysqld.service文件中添加参数AmbientCapabilities=CAP_SYS_NICE启用它**
- **对于不支持systemd方式的操作系统，可以将mysqld文件打开它setcap cap_sys_nice+ep /path/to/mysqld**

##### 不管使用那种方式开启CAP_SYS_NICE功能，都需要重启MySQL

##### 至此，关于MySQL 8.0资源组功能就介绍完了，小伙伴们可以根据自己的业务需求开启它的奇妙之旅吧！
