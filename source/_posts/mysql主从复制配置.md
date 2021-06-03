---
title: mysql主从复制配置
language: zh-CN
date: 2021-06-03 15:53:41
tags:
- mysql
- 主从复制
---
#### 一、数据库配置
##### 1、主数据库配置
- my.cnf 增加mysqld配置
```
[mysqld]
#设置server-id
server-id=100
#开启二进制日志
log-bin=mysql-bin

binlog-ignore-db=mysql

binlog_cache_size=1M

binlog_formate=mixed
```
##### 2、从数据库配置
- my.cnf 增加mysqld配置
```
[mysqld]
#设置server-id
server-id=102
#开启二进制日志
log-bin=mysql-slave-bin

relay_log=edu-mysql-relay-bin

log_bin_trust_function_creators=true

binlog-ignore-db=mysql

binlog_cache_size=1M

binlog_formate=mixed

slave_skip_errors=1062
```
##### 3、分别重启主从数据库

#### 二、数据库开启主从复制
##### 1、主数据库执行命令
- 授予账号的访问权限
```
grant replication slave,replication client on *.* to 'root'@'192.168.0.152' identified by '123456';
flush privileges;
```
- 查看主数据库复制信息
```
show master status;
```

![Image text](./1.png)

2、从数据库执行命令
- 配置从服务器关于主服务器的信息
```
change master to master_host='192.168.0.152', master_user='root',master_password='123456',master_port=3307,master_log_file='mysql-bin.000001',master_log_pos=613;
```
- 开始主从复制
```
start slave;
```
- 查看主从复制的状态
```
show slave status\G;
```
![Image text](./2.png)