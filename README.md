# MySQL

## 概述

MySQL 是全球最流行的开源关系型数据库管理系统（RDBMS）之一，由 Oracle 公司开发和维护。它以其**高性能、高可靠性、易用性和开源免费**的特性，成为 Web 应用程序中最常用的数据库解决方案。

## 核心特性

- **开源免费**：基于 GPL 许可证，社区版可免费使用
- **跨平台支持**：支持 Linux、Windows、macOS 等多种操作系统
- **ACID 兼容**：支持事务处理，保证数据一致性
- **复制与高可用**：提供主从复制、组复制等解决方案
- **存储引擎架构**：支持 InnoDB、MyISAM 等多种存储引擎
- **强大的生态系统**：丰富的工具链和社区支持

## 主要应用场景

| 场景 | 说明 | 优势 |
|------|------|------|
| **Web 应用** | LAMP/LEMP 技术栈的核心组件 | 高性能读写，易于扩展 |
| **数据仓库** | OLAP 业务分析 | 支持复杂查询和聚合操作 |
| **嵌入式系统** | 嵌入式数据库解决方案 | 轻量级，资源占用少 |
| **云原生应用** | 容器化部署 | 良好的 Kubernetes 生态支持 |

## 快速开始

### 安装 MySQL

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql
sudo systemctl enable mysql
