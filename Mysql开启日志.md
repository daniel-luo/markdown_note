##Mysql Log##
**my.cng**
```ini
[mysqld]
general_log_file = /path/to/query.log
general_log      = 1

```
**SQL**
开启
```sql
SET GLOBAL general_log = 'ON';
```

关闭
```sql
SET GLOBAL general_log = 'OFF';
```

Ubuntu 16 的 Mysql 的配置文件在 `/etc/mysql/mysql.conf.d/mysqld.cnf`


##Documents##
[https://dev.mysql.com/doc/refman/5.7/en/query-log.html](https://dev.mysql.com/doc/refman/5.7/en/query-log.html)