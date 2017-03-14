1. 创建账户和密码
CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';
2。
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
3.
CREATE USER 'root'@'%' IDENTIFIED BY 'password';
4.
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
5.
FLUSH PRIVILEGES;

```sql
GRANT ALL ON *.* to daniel@'172.17.0.1' IDENTIFIED BY '12345abc';
FLUSH PRIVILEGES;
```

##Documents##
[http://stackoverflow.com/questions/1559955/host-xxx-xx-xxx-xxx-is-not-allowed-to-connect-to-this-mysql-server](http://stackoverflow.com/questions/1559955/host-xxx-xx-xxx-xxx-is-not-allowed-to-connect-to-this-mysql-server	)