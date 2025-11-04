# MySQL 安装

# 在开始本文操作之前，有以下几点需要声明
** 本文中的操作系统为oracle linux 9.6，兼容centos、redhat **
** 本文假设已安装好操作系统，包括时钟、主机名等操作
** 放开了默认SSH 22端口，可远程登陆
** 本文采用二进制压缩包的方式安装
** 本文操作系统采用X86_64架构
** 本文MySQL版本：8.4.7 LTS（LTS：Long-Term Support，长期支持版本）

# 开始正文

第一步：下载MySQL 软件包
```bash
wget https://dev.mysql.com/get/Downloads/MySQL-8.4/mysql-8.4.7-linux-glibc2.28-x86_64.tar.xz


第二步：解压
