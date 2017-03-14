##mysql: [ERROR] Found option without preceding group in config file##
Add `[mysqld]` as a first line of your my.cnf-file.



**Mysql add log file**
```shell
general_log_file        = /home/daniel/mysql/mysql.log
general_log             = 1
```



##Documents##
[http://blog.programster.org/ubuntu-16-04-default-mysql-5-7-configuration/](http://blog.programster.org/ubuntu-16-04-default-mysql-5-7-configuration/)
[http://serverfault.com/questions/471372/etc-my-cnf-what-i-am-missing](http://serverfault.com/questions/471372/etc-my-cnf-what-i-am-missing)