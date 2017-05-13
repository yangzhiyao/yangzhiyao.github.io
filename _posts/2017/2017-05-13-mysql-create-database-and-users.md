# MySQL和MariaDB创建数据库和用户的常用命令

之前一直使用phpmyadmin来完成这方面的事情，尽管一些常用的增删改查不需要查文档，但是每次建数据库和分配用户权限的时候还要在搜索引擎里面找。

## 创建数据库时使用utf8字符编码

```sql
create database db_name character set utf8;
```

## 修改数据库编码的命令

```sql
alter database app_relation character set utf8;
```

## 创建用户

```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

## 授权用户权限
```sql
GRANT privileges ON databasename.tablename TO 'username'@'host';
```

## 回收用户权限
```sql
REVOKE privilege ON databasename.tablename FROM 'username'@'host';
```

## 用户密码的设置与更改
```sql
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
```

## 删除用户
```sql
DROP USER 'username'@'host';
```
