# MySQL 执行计划

#### 通过MySQL 执行计划，可以很直观的看到SQL的性能，并进行调试和修改，它是了解SQL好坏的最直接工具，也是最常用的方式之一

#### MySQL 执行计划类型

traditional：表格格式，默认格式  
```
explain select col1,col2 from t1 where t1.col1=100;
```

json：json格式，内容详细  
```
explain format=json select col1,col2 from t1 where t1.col1=100;
```

tree：树形格式，按照执行顺序层级展示  
```
explain format=tree select col1,col2 from t1 where t1.col1=100;
```

analyze：采用树形格式，它是执行计划+真实执行，结果会显示执行时间  
```
explain analyze select col1,col2 from t1 where t1.col1=100;
```

##### 也支持通过语法"EXPLAIN FOR CONNECTION ID"语法，显示执行计划
##### 通过"show warnings;"，可以查看重写后的SQL

