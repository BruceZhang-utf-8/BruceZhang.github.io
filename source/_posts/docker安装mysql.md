---
title: docker安装mysql
date: 2023-08-30 22:26:28
tags:
- docker
- mysql
categories:
- docker
---

# docker 安装mysql

## 1.下载镜像文件

```shell
docker pull mysql:5.7
```

## 2.创建实例并启动

```shell
docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql/conf.d \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql:5.7
#参数说明
-p 3306:3306：将容器的 3306 端口映射到主机的 3306 端口
-v /mydata/mysql/conf:/etc/mysql/conf.d 将配置文件夹挂载到主机
-v /mydata/mysql/log:/var/log/mysql：将日志文件夹挂载到主机
-v /mydata/mysql/data:/var/lib/mysql/：将配置文件夹挂载到主机
-e MYSQL_ROOT_PASSWORD=root：初始化 root 用户的密码


# MySQL 配置
vim /mydata/mysql/conf/my.cnf
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve

# 查看镜像
docker images

# 查看所有容器
docker ps -a
# 启动容器
docker start 容器id
```

**注意：解决mysql连接慢的问题**

```
[mysqld]
skip-name-resolve
# 解释
skip-name-resolve：跳过域名解析
```

## 3.通过容器的mysql命令行链接

```
docker exec -it mysql mysql -uroot -p123456
```

## 4.设置root远程访问

```
grant all privileges on *.* to 'root'@'%' identified by '用户密码' with grant option;
flush privileges;
```

## 5.进入容器文件系统

```shell
docker exec -it mysql /bin/bash
```





