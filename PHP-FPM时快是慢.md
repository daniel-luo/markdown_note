##PHP-FPM时快是慢
**1. php-fpm修改默认设置**
```conf
pm.max_children = 300
pm.start_servers = 15
pm.max_spare_servers = 4

; 开启慢查询
request_slowlog_timeout = 10s
slowlog = /home/daniel/python/tmp/$pool.log
```

**2. cpu 默认读写**
```shell
ulimit -u
# 临时修改
ulimit -SHn 51200
```

##Documents
[http://motion.mez100.com.cn/](http://motion.mez100.com.cn/)
[http://blog.csdn.net/bugall/article/details/45869183](http://blog.csdn.net/bugall/article/details/45869183)
[http://www.linuxidc.com/Linux/2013-07/86963.htm](http://www.linuxidc.com/Linux/2013-07/86963.htm)
[ulimit 修改](https://jingyan.baidu.com/article/656db918d36b24e381249cb1.html)
