# MySQL 用户与SSL

#### MySQL 从8.0开始在用户安全、传输安全方面，进行了很多的改进，下面将对这些改进进行系统性梳理

- **密码验证插件由mysql_native_password改为caching_sha2_password**
- **增加了密码强度验证插件validate_password**
- **增加了角色功能**
- **增加了用户与用户之间授权功能**
- **增加了部分权限回收功能**
- **增加了同一账户双密码功能，以及可以使旧密码优雅的下线功能**
- **增加了多重密码验证功能**
- **服务端默认开启SSL，但不强制客户端开启SSL**

创建用户，并授权
```
CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'password';
GRANT SELECT ON app_db.* TO 'jeffrey'@'localhost';
```

查看用户及权限
##### 参数：“print_identified_with_as_hex”是将hash后的不可打印字符转化为十六进制字符串
```
set print_identified_with_as_hex = on;
SHOW CREATE USER 'jeffrey'@'localhost';
SHOW GRANTS FOR 'jeffrey'@'localhost';
select * from mysql.user where user='jeffrey' and host='localhost';
```

修改用户密码
```
ALTER USER 'jeffrey'@'localhost' IDENTIFIED BY 'password';
```

修改用户密码，并保留原密码
```
ALTER USER 'jeffrey'@'localhost' IDENTIFIED BY 'password' retain current password;
```

删除旧密码
```
alter user 'jeffrey'@'localhost' discard old password;
```

删除用户
```
drop user 'jeffrey'@'localhost';
```

查看密码验证插件参数
```
mysql> SHOW VARIABLES LIKE 'validate_password%';
+-------------------------------------------------+--------+
| Variable_name                                   | Value  |
+-------------------------------------------------+--------+
| validate_password.changed_characters_percentage | 0      |
| validate_password.check_user_name               | ON     |
| validate_password.dictionary_file               |        |
| validate_password.length                        | 8      |
| validate_password.mixed_case_count              | 1      |
| validate_password.number_count                  | 1      |
| validate_password.policy                        | MEDIUM |
| validate_password.special_char_count            | 1      |
+-------------------------------------------------+--------+
8 rows in set (0.01 sec)
```

- **caching_sha2_password插件即使非SSL连接环境下，仍然可以用RSA 密钥对，进行客户端与服务端的密码交换，防止密码泄露**
- **caching_sha2_password插件使用SHA256(SHA256())算法，即使相同的密码，也会得到不同的hash值，使得预先计算的彩虹表攻击失效，每个密码都需要单独破解，且支持缓存，支持更多并发，尤其是连接池**
- **MySQL 8.0版本开始服务端默认开启SSL，客户端为了兼容性，不强制开启，若客户端强制开启，受参数“require_secure_transport”控制**
- **MySQL 8.0新增的角色功能与账户平级，在实际使用中，可以根据需要创建，个人认为如果不习惯角色功能带来的管理不便，可以保留原来直接授权的方式**
- **使用SSL加密传输，会加大对网络和MySQL 实例CPU的压力，使QPS降低10%左右，请谨慎使用，可以对应用接入层使用HTTPS，MySQL只内网白名单访问**
- **MySQL 主从、组复制，支持SSL，但默认不开启，建议不开启**
- **在实际生产环境中，可以根据需要设置密码生存周期，以及最近几次修改密码不能相同等安全措施**

#### 参数链接：https://dev.mysql.com/doc/refman/8.4/en/security.html

#### 至此关于MySQL 用户与SSL的用法和功能就介绍完成了，小伙伴们根据需要用起来吧！
