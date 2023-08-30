---
title: mysql问题集锦
date: 2023-08-31 07:16:41
tags:
- mysql
- mysql问题集锦
categories:
- mysql
---

# mysql问题集锦

## 一 mysql使用localhost可以连接成功，但是ip不能连接

### 问题描述

mysql使用localhost或127.0.0.1可以连接成功，但是使用ip地址连接失败

### 问题解决

1) 命令行进入mysql

   ```shell
   # 使用root用户进入
   mysql -uroot -p123456
   ```

2) 查看用户和对应的主机

   ```sql
   select host,user from mysql.user;
   ```

![image-20230831074341595](mysql问题集锦/image-20230831074341595.png)

​        root用户的host为localhost,需要授权

3. 分配新用户

   ```sql
   grant all privileges on *.* to '用户名'@'IP地址' identified by '密码';
   
   # 'all privileges ':所有权限 也可以写成 select ,update等。
   # *.* 所有库的所有表 如 databasename.*。
   # IP  数据库所在的IP。
   # identified by ‘密码’ 表示通过密码连接。
   ```

   

4. 刷新权限

   ```sql
   flush privileges;
   ```

5. 修改my.cnf配置

   ```
   [mysqld] ... bind_address=127.0.0.1 # 屏蔽掉该处 ...
   ```

6. 重新启动mysql

   ```shell
   service stop mysql
   service start mysql
   ```

   
