# MySQL 安装

## 在开始本文操作之前，有以下几点需要声明
- **本文中的操作系统为oracle linux 9.6，兼容centos、redhat**  
- **本文假设已安装好操作系统，包括时钟、主机名等操作**  
- **放开了默认SSH 22端口，可远程登陆**  
- **本文采用二进制压缩包的方式安装**  
- **本文操作系统采用X86_64架构**  
- **本文MySQL版本：8.4.7 LTS（LTS：Long-Term Support，长期支持版本）**  

## 开始正文

下载MySQL 软件包
```
wget https://dev.mysql.com/get/Downloads/MySQL-8.4/mysql-8.4.7-linux-glibc2.28-x86_64.tar.xz
```

解压至指定目录
```
tar -xvf mysql-8.4.7-linux-glibc2.28-x86_64.tar.xz -C /opt/
```

重命名目录名称
```
mv mysql-8.4.6-linux-glibc2.28-x86_64/ mysql-8.4.6  
```

创建组、用户
```
groupadd mysql  
useradd -r -g mysql -s /bin/false mysql  
```

创建数据目录，并授权、修改权限  
```
mkdir -p /opt/data  
chown mysql:mysql /opt/data  
chmod 750 /opt/data
```

创建my.cnf并编辑  
```
vi /etc/my.cnf
[client]
socket=/opt/data/mysql.sock

[mysqld]
basedir=/opt/mysql-8.4.6
datadir=/opt/data
```

配置全局系统变量，并生效  
```
vi /etc/profile
export PATH=$PATH:/opt/mysql-8.4.6/bin

source /etc/profile
```

初始化MySQL
```
mysqld --initialize --user=mysql
```

##### 特别提示：若报错，可根据报错提示修复，常见报错如下：
1.目录权限不足，需授权  
2.缺少依赖包，比如：libaio包、glibc库版本低  
3.反复初始化后，数据目录非空，需清空  


设置MySQL开机自启动
```
cp /opt/mysql-8.4.6/support-files/mysql.server /etc/init.d/
mv /etc/init.d/mysql.server /etc/init.d/mysql
vi /etc/init.d/mysql
将如下变量地址修改成安装地址
basedir=/opt/mysql-8.4.6
datadir=/opt/data

chkconfig --add mysql
chkconfig --list
```

启动MySQL
```
service mysql status
service mysql start
```

登录MySQL，修改root@localhost权限密码  
```
mysql -uroot -p
alter user 'root'@'localhost' identified by 'mysql';
```
