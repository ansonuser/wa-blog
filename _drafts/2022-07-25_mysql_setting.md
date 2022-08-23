---
title: MySQL setting
categories: Computer
---


新增遠端使用者連線


sql_bash>>

```
$ create user 'USERNAME'@'IP' identified by '$PWD';

$ flush privileges; 

$ grant all privileges on $database_name.* to 'USERNAME'@'IP';  
or
$ grant all privileges on *.* to 'USERNAME'@'IP'; # 所有權限

$ select user, host from mysql.usr;


----------------------
| user           |  host          |
|--------------------  |
| root           | localhost |
| username   |  IP              |
----------------------

```


bash>>
```
$ sudo iptables -A INPUT -p tcp --destination-port 3306 -j ACCEPT
//關閉防火牆的連線限制, 指定特定ip用-s的flag
```