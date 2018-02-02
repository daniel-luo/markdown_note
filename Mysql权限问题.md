##Mysql权限问题
权限问题，授权 给 root  所有sql 权限
```shell
mysql> grant all privileges on *.* to root@"%" identified by ".";
Query OK, 0 rows affected (0.00 sec)


mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```