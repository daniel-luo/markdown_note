##Apache2.4.16 配置 PHP-FPM##
1. 打开以下模块

```ini
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so
```

2. 关闭以下模块,如果有

```ini
#LoadModule fcgid_module modules/mod_fcgid.so
```

3. 配置 php-fpm
 - 到 php 安装目录,　这里目录是： `/opt/data/php/7.0.7/etc`
 - `cp php-fpm.conf.default php-fpm.conf`
 - `cd php-fpm.d`
 - `cp www.conf.default www.conf`
 - 修改 `www.conf`

```ini
user = daemon
group = daemon
# 如果有多个版本的 PHP 可以改为不同的端口
listen = 127.0.0.1:9000
```

4. 启动 PHP-FPM `sudo /opt/data/php/7.0.7/sbin/php-fpm`

5. 配置 vhost, 这里需要注意 `ProxyPassMatch`
 - `"^/.*(/.php)?$"` 表示所有的访问使用这个代理
 - `"fcgi://localhost:9000/opt/data/apache/use/htdocs/7.0.7/local.hollywood.com/public_html/"`,其中 `fcgi://localhost:9000` 是代理端口, `/opt/data/apache/use/htdocs/7.0.7/local.hollywood.com/public_html/` 是项目根目录

**配置方法一**

```ini
<VirtualHost *:80>
    ServerAdmin support@tenfsoftware.com
    DocumentRoot "/opt/data/apache/use/htdocs/7.0.7/local.hollywood.com/public_html"
    ServerName local.hollywood.com

    ProxyRequests Off
    ProxyPassMatch "^/.*(/.*)?$" "fcgi://localhost:9000/opt/data/apache/use/htdocs/7.0.7/local.hollywood.com/public_html/" enablereuse=on
 
    <Directory /opt/data/apache/use/htdocs/7.0.7/local.hollywood.com/public_html>
        Options +ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>

    ErrorLog "logs/local.hollywood.com-error_log"
    CustomLog "logs/local.hollywood.com-access_log" common
</VirtualHost>
```

**配置方法二**

```ini
<VirtualHost *:80>
    ServerAdmin support@tenfsoftware.com
    ServerName local.hollywood.com

    DocumentRoot "/opt/data/apache/use/htdocs/7.0.7/local.hollywood.com/public_html"

    <Directory /opt/data/apache/use/htdocs/7.0.7/local.hollywood.com/public_html>
        Options -Indexes +FollowSymLinks +MultiViews
        AllowOverride All
        Require all granted
    </Directory>

    ProxyRequests Off
    <FilesMatch \.php$>
        # 2.4.10+ can proxy to unix socket
        # SetHandler "proxy:unix:/var/run/php5-fpm.sock|fcgi://localhost/"

        # Else we can just use a tcp socket:
        #SetHandler "proxy:fcgi://127.0.0.1:9000"
        SetHandler "proxy:fcgi://127.0.0.1:9000"
    </FilesMatch>

    #ErrorLog ${APACHE_LOG_DIR}/example.com-error.log
    ErrorLog "logs/local.hollywood.com-error_log"

    # Possible values include: debug, info, notice, warn, error, crit,
    # alert, emerg.
    #LogLevel warn

    #CustomLog ${APACHE_LOG_DIR}/example.com-access.log combined
    #CustomLog "logs/local.hollywood.com-access_log" common

</VirtualHost>
```
**注意:** `<FilesMatch \.php$>` 这儿不能写 `.*` 因为 `/opt/data/php/7.0.7/etc/php-fpm.d/www.conf` 这个配置文件里有做解析限制 `security.limit_extensions`, php-fpm 只会解析这里指定后缀名的文件


##PHP-FPM 控制##
1. php-fpm 启动
- `sudo /usr/local/php/sbin/php-fpm`
2. php-fpm 关闭：
- `kill -INT  php-fpm_main_process_id`　Or `kill -INT 'cat /var/run/php-fpm/php-fpm.pid'`
3. php-fpm 重启：
- `kill -USR2  php-fpm_main_process_id`　Or `kill -USR2 'cat /var/run/php-fpm/php-fpm.pid'`

**master进程可以理解以下信号**
- INT, TERM 立刻终止
- QUIT 平滑终止	
- USR1 重新打开日志文件
- USR2 平滑重载所有worker进程并重新载入配置和二进制模块

**资料**
[http://www.111cn.net/phper/php-gj/52906.htm](http://www.111cn.net/phper/php-gj/52906.htm)


##PHP-FPM CPU长时间占用100%##
1. 进程跟踪 `top` 找出CPU使用率高的进程PID
2. `strace -p PID` 查看这个进程在做啥
3. `ll /proc/PID/fd` 查看该进程在处理哪些文件
4. `pmap $(pgrep php-cgi |head -1)` 通过pmap指令查看PHP-CGI进程的内存使用情况,一个较为干净的PHP-CGI打开大概20M-30M左右的内存
**资料**
[http://blog.csdn.net/zhuoxiong/article/details/8563256](http://blog.csdn.net/zhuoxiong/article/details/8563256)

##Documents##
[http://httpd.apache.org/docs/trunk/mod/mod_proxy_fcgi.html#page-header](http://httpd.apache.org/docs/trunk/mod/mod_proxy_fcgi.html#page-header)(官方)
[https://serversforhackers.com/video/apache-and-php-fpm](https://serversforhackers.com/video/apache-and-php-fpm) (更好的虚拟主机配置)
[https://wiki.apache.org/httpd/PHP-FPM](https://wiki.apache.org/httpd/PHP-FPM)(PHP-FPM配置)
[http://blog.csdn.net/reblue520/article/details/50405148](http://blog.csdn.net/reblue520/article/details/50405148)
[http://cnzhx.net/blog/apache-httpd-mod_proxy_fcgi-php-fpm/](http://cnzhx.net/blog/apache-httpd-mod_proxy_fcgi-php-fpm/)
[http://www.iyunv.com/thread-22961-1-1.html](http://www.iyunv.com/thread-22961-1-1.html)

**多版本共存提示**
[http://www.oschina.net/question/1271022_2152523](http://www.oschina.net/question/1271022_2152523)

**PHP-FPM 控制**
[serverfault.com/questions/189940/how-do-you-restart-php-fpm](serverfault.com/questions/189940/how-do-you-restart-php-fpm)

##Module address##
[http://httpd.apache.org/mod_fcgid/](http://httpd.apache.org/mod_fcgid/)


##Timeout Error##
[http://serverfault.com/questions/500467/apache2-proxy-timeout](http://serverfault.com/questions/500467/apache2-proxy-timeout)
[http://httpd.apache.org/docs/current/mod/mod_proxy_fcgi.html](http://httpd.apache.org/docs/current/mod/mod_proxy_fcgi.html)
[http://httpd.apache.org/docs/current/mod/mod_proxy.html](http://httpd.apache.org/docs/current/mod/mod_proxy.html)
[https://talk.plesk.com/threads/problem-php-fpm-70007-the-timeout-specified-has-expired-ah01075-er.335211/](https://talk.plesk.com/threads/problem-php-fpm-70007-the-timeout-specified-has-expired-ah01075-er.335211/)
[http://stackoverflow.com/questions/31975951/proxy-ajperror-70007the-timeout-specified-has-expired](http://stackoverflow.com/questions/31975951/proxy-ajperror-70007the-timeout-specified-has-expired) (solve)