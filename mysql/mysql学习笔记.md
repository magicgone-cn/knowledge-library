# 安装

## 使用docker安装

> 快速启动
```sh
docker container run --name mysql -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=a -dp 3306:3306 mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --lower_case_table_names=1 --transaction_isolation=read-committed
```

> docker-compose
```yaml
mysql-8.0:
  image: mysql:8.0
  environment:
    MYSQL_ROOT_PASSWORD: SINObest@2020
  ports:
    - 3306:3306
  volumes:
    - mysql-8.0:/var/lib/mysql
  command: mysqld --lower_case_table_names=1
```

# 启动参数

```bash
#表名忽略大小写
--lower_case_table_names=1
#
--innodb_flush_log_at_trx_commit=0 
#
--sync_binlog=0
```

## 常用命令

```mysql
-- 创建用户
create user hsa identified by 'SINObest@2020';

-- 创建数据库并授权
create database macrodcs_adb_db character set utf8mb4 collate utf8mb4_general_ci;
GRANT ALL PRIVILEGES ON macrodcs_adb_db.* TO hsa;
```

# 事务隔离级别

```mysql
-- 查看事务隔离级别
select @@tx_isolation;
-- 查看全局事务隔离级别
select @@global.tx_isolation;
-- 设置事务隔离级别
set transaction isolation level read committed;
-- 设置会话事务隔离级别
set session transaction isolation level read committed;
-- 设置全局事务隔离级别
set global transaction isolation level read committed;
```

# sql_mode

3065 - Expression #1 of ORDER BY clause is not in SELECT list, references column which is not in SELECT list; this is incompatible with DISTINCT

```sql
SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY,',''));
SET SESSION sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY,',''));
```

默认的sql_mode=ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
