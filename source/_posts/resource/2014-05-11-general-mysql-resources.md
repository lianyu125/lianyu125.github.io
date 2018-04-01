---
title: MySQL常用资源
category: MySQL
tags: MySQL
keywords: MySQL
description:  MySQL常用资源
---
## 安装数据库
 1、先到mysql官网上下载dmg格式安装包，进行安装。
 2、安装完后，命别名：
 
```bash
alias mysql=/usr/local/mysql/bin/mysql
alias mysqladmin=/usr/local/mysql/bin/mysqladmin
```
## 给root创建密码：
```
/usr/local/mysql/bin/mysqladmin -u root password root
```
## 通过shell连接到数据库
cd /usr/local/mysql
启动sudo support-files/mysql.server start
重启sudo support-files/mysql.server restart
停止sudo support-files/mysql.server stop

 2.连接到数据库:`mysql -h localhost -uroot -p`
 

### 登录数据库
    mysql -h localhost -uroot -p

### 导出数据库 
    mysqldump -uroot -p db > db.sql
### 导入数据库 
    mysql -uroot -p db < db.sql
    // or
    mysql -uroot -p db -e "source /path/to/db.sql"
### 开启远程登录
    grant all privileges on ss.* to 'root'@'%' indentified by 'passoword' with grant option;
    // or 
    update user set Host="%" and User="root"
    // 注意%是不包含localhost的
    flush privileges;
    
### 创建用户
    
    CREATE USER 'test'@'localhost' IDENTIFIED BY 'password';
    grant all privileges on *.* to test@'localhost' identified by 'test';
    
### 创建表
    
    CREATE SCHEMA testdb DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;

### 赋予数据库权限

    GRANT ALL ON testdb.* TO 'test'@'localhost';
    
### 其他常用命令
    show databases; //显示所有的数据库
    create database chinacity; //创建名为chinacity的数据库
    drop database chinacity; //删除名为chinacity的数据库
    use  mysql;      //使用mysql数据库
    show tables from mysql; //查看某个数据库中的表
    

