##Dockedr PHP环境部署##
Author: Daniel luo
email: luo3555@qq.com

[1. 安装Docker](#安装Docker)
[2. Docker基本命令](#Docker基本命令)
[3. 搭建Nginx + PHP + Mysql环境](#搭建Nginx + PHP + Mysql环境)
[4. 相关文档](#相关文档)

***

###<span id="安装Docker">1. 安装Docker</span>###
[安装步骤](http://www.docker.org.cn/book/install/win7-win8-install-docker-40.html)
[国内下载地址](https://get.daocloud.io/toolbox/)
[https://www.docker.com/community-edition#download](https://www.docker.com/community-edition#download)
[https://download.daocloud.io/Docker_Mirror/Docker](https://download.daocloud.io/Docker_Mirror/Docker)

***

###<span id="Docker基本命令">2. Docker基本命令</span>###
1. 根据`Dockerfile`创建镜像
```shell
docker build -t 自定义镜像名字:自定义镜像版本 .
```
,注意最后结尾有一个空格加`.`

2. 使用 `cocker-compose` 运行容器
```shell
docker-compose -f 你定义的yml文件 up
```
3. 查看有哪些容器
```shell
docker ps -a
```

4. 进入到容器
```shell
docker exec -it 容器第三步里得到的容器ID /bin/bash
```

5. 删除现有容器
```shell
docker rm $(docker ps -a -q)
```

6. 删除所有镜像
```shell
docker rmi $(docker images -q)
```

7. 查看容器有哪些可挂在的点
```shell
docker inspect container_name | grep Mounts -A 20
```

8. docker-compose 启动容器
```shell
docker-compose -f php-env.yml up -d
```

9. 定义Tag
```shell
docker tag image_name:tag repository_name
```
eg:`docker tag alpine:3.4 firewarm/alpine:3.4`
注意:通常`repository_name`是由你的`DockerId/TAG`组成的

10. 推送本地镜像到DockerHub
```shell
docker login
docker push YOUR_DOCKER_ID/LOCAL_IMAGE:TAG
```

11. 已非root的方式启动容器,在运行前添加用户并将用户切换到新加的用户下
```shell
RUN useradd noroot -u 1000 -s /bin/bash
USER noroot
```

***

###<span id="搭建Nginx + PHP + Mysql环境">3. 搭建Nginx + PHP + Mysql环境</span>###
创建环境时的文件夹结构
```text
Dockerfile
sources.list.trusty
supervisord.conf
default
phpenv.yaml
```
1. Dockerfile 创建自定的web服务器用到的Dockerfile文件
2. sources.list.trusty 163的ubuntu 更新源，不换会很慢
3. supervisord.conf supervisord的配置文件，配置随机启动的程序
4. default Nginx的主机的配置文件
5. phpenv.yaml 启动容器时的YAML文件


####3.1. Nginx + PHP Dockerfile####
1. 创建自己的PHPweb服务器Dockerfile, 该Dockerfile修改自`Andrei Susanu <andrei.susanu@gmail.com>`的Dockerfile

**Dockerfile**
```Dockerfile
FROM daocloud.io/library/ubuntu:trusty-20170330

MAINTAINER Daniel Luo <luo3555@qq.com>

ENV DEBIAN_FRONTEND noninteractive

# remove origin sources.list add 163 sources
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak

COPY sources.list.trusty /etc/apt/sources.list

# add NGINX official stable repository
RUN echo "deb http://ppa.launchpad.net/nginx/stable/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/nginx.list

# add PHP5.6 unofficial repository (https://launchpad.net/~ondrej/+archive/ubuntu/php)
RUN echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/php.list

# add public key
RUN apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 00A6F0A3C300EE8C
RUN apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 4F4EA0AAE5267A6C

# install packages
RUN apt-get update && \
    apt-get -y --force-yes --no-install-recommends install \
    supervisor \
    nginx \
    php5.6-cli php5.6-fpm php5.6-common php5.6-mysql \
    php5.6-curl php5.6-xml php5.6-zip php5.6-soap php5.6-xsl \
    php5.6-sqlite3 php5.6-gd php5.6-json php5.6-mcrypt php5.6-intl php5.6-imap php5.6-mbstring

RUN apt-get -y --force-yes --no-install-recommends install \
    php7.0-cli php7.0-fpm php7.0-common php7.0-mysql \
    php7.0-curl php7.0-xml php7.0-zip php7.0-soap php7.0-xsl \
    php7.0-sqlite3 php7.0-gd php7.0-json php7.0-mcrypt php7.0-intl php7.0-mbstring \
    php7.0-bcmath

RUN apt-get -y --force-yes --no-install-recommends install \
    php7.1-cli php7.1-fpm php7.1-common php7.1-mysql \
    php7.1-curl php7.1-xml php7.1-zip php7.1-soap php7.1-xsl \
    php7.1-sqlite3 php7.1-gd php7.1-json php7.1-mcrypt php7.1-intl php7.1-mbstring \
    php7.1-bcmath

# configure NGINX as non-daemon
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# configure php-fpm as non-daemon
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/5.6/fpm/php-fpm.conf

# clear apt cache and remove unnecessary packages
RUN apt-get autoclean && apt-get -y autoremove

# add a phpinfo script for INFO purposes
RUN echo "<?php phpinfo();" >> /var/www/html/index.php

# NGINX mountable directories for config and logs
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx"]

# NGINX mountable directory for apps
VOLUME ["/var/www"]

# copy config file for Supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
# Superisor mountable directory for config
VOLUME ["/etc/supervisor/conf.d"]

# backup default default config for NGINX
RUN cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak

# copy local defualt config file for NGINX
COPY default /etc/nginx/sites-available/default

# php-fpm will not start if this directory does not exist
RUN mkdir /run/php

# NGINX ports
EXPOSE 80 443

CMD ["/usr/bin/supervisord"]
```
**sources.list.trusty**

```text
deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse
```

**default**

```text
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;

    index index.php index.html;

    server_name _;

    access_log /var/log/nginx/default.access.log;
    error_log /var/log/nginx/default.error.log;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php5.6-fpm.sock;
    }
}
```

**supervisord.conf**
```text
[supervisord]
nodaemon=true

[program:php5.6-fpm]
command=/usr/sbin/php-fpm5.6

[program:php7.0-fpm]
command=/usr/sbin/php-fpm7.0

[program:php7.1-fpm]
command=/usr/sbin/php-fpm7.1

[program:nginx]
command=/usr/sbin/nginx
```
**php-env.yml**

```yml
version: '2.0'

services:
    db:
      image: 'daocloud.io/library/mysql:5.6.17'
      environment:
          MYSQL_ROOT_PASSWORD: 12345abc
          MYSQL_ALLOW_EMPTY_PASSWORD: 0
      ports:
          - 3306:3306
      volumes:
            - /Users/Daniel.luo/www/mysql:/var/lib/mysql

    nginx:
      image: 'nginx-php:1.0'
      ports:
          - 80:80
          - 443:443
      links:
          - db:db
      depends_on:
          - db
      volumes:
          - /Users/Daniel.luo/www:/var/www
          - /Users/Daniel.luo/www/nginx/sites-enabled:/etc/nginx/sites-enabled
          - /Users/Daniel.luo/www/nginx/log:/var/log/nginx
```
**使用你的路径替换**`/Users/Daniel.luo/www`

***

##其他配置


Magento1.x vhost

```conf
server {
    listen 80;
    server_name local.example.com; #修改为你的服务器名
    access_log  /var/log/nginx/magento1924.access.log;
    error_log   /var/log/nginx/magento1924.error.log;
    location / {
        root /var/www/html/local.example.com/public_html;
        index  index.php index.html index.htm;
       # rewrite ^(/index.php)?/minify/([^/]+)(/.*.(js|css))$ /lib/minify/m.php?f=$3&d=$2 last;
       #上面的这条Rewrite规则是为了性能优化，安装fooman-speedster插件时需要的， 如果你没有安装这个插件，请注释掉这条规则。

        if (-f $request_filename) {
               expires 30d;
               break;
        }
        if (!-e $request_filename) {
               rewrite ^(.+)$ /index.php last;
        }
        #上面的两条Rewrite规则可以确保Magento在Nginx下完成正常Rewrite工作。
        location ~* \.(ico|jpg|jpeg|png|gif|svg|js|css|swf|eot|ttf|otf|woff|woff2)$ {
        add_header Cache-Control "public";
        add_header X-Frame-Options "SAMEORIGIN";
        expires +1y;
        if (!-f $request_filename) {
                rewrite ^/pub/static/(version\d*/)?(.*)$ /pub/static.php?resource=$2 last;
        }
    }
    location ~* \.(zip|gz|gzip|bz2|csv|xml)$ {
        add_header Cache-Control "no-store";
        add_header X-Frame-Options "SAMEORIGIN";
        expires    off;
        if (!-f $request_filename) {
            rewrite ^/pub/static/(version\d*/)?(.*)$ /pub/static.php?resource=$2 last;
        }
    }
    #location ~ \.php$ {
        #fastcgi_pass   127.0.0.1:9001;
        #fastcgi_index  index.php;
        #fastcgi_param  SCRIPT_NAME $fastcgi_script_name;
        #fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;  #修改为你的Magento目录夹
        #include        fastcgi_params;  #请检查fastcgi_params文件是否存在， 默认是有的
    #}

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php5.6-fpm.sock;
    }

    location /api {
        rewrite ^/api/rest /api.php?type=rest last;
        rewrite ^/api/v2_soap /api.php?type=v2_soap last;
        rewrite ^/api/soap /api.php?type=soap last;
    }
    location /app/etc {
        deny all;
    }
    #上面这条规则禁止访问/app/etc/目录夹，防止别人非法读取配置文件，得到密码等信息
    }
    ## These locations would be hidden by .htaccess normally
    location ^~ /app/                { deny all; }
        location ^~ /includes/           { deny all; }
        location ^~ /lib/                { deny all; }
        location ^~ /media/downloadable/ { deny all; }
        location ^~ /pkginfo/            { deny all; }
        location ^~ /report/config.xml   { deny all; }
        location ^~ /var/                { deny all; } 
        location  /. { ## Disable .htaccess and other hidden files
            return 404;
        }
    location @handler { ## Magento uses a common front handler
            rewrite / /index.php;
        }
    location ~ .php/ { ## Forward paths like /js/index.php/x.js to relevant handler
            rewrite ^(.*.php)/ $1 last;
        }
    location ~ .php$ { ## Execute PHP scripts
            rewrite / /index.php last; 
    }
    location ~* (\.htaccess$|\.git) {
            deny all;
    }
gzip on;
#gzip_http_version 1.1;
#gzip_vary on;
gzip_disable "msie6";
gzip_comp_level 6;
gzip_min_length 700;
gzip_buffers 16 8k;
gzip_proxied any;
gzip_types
text/plain
text/css
text/js
text/xml
text/javascript
application/x-httpd-php
application/javascript
application/x-javascript
application/json
application/xml
application/xml+rss
image/svg+xml
image/jpeg
image/gif
image/png;
gzip_vary on;
}

```

Magento2 vhost
```conf
server {
    listen 80;    server_name local.magento2.com;
    set $MAGE_ROOT /var/www/html/local.magento2.com/public_html;
    set $MAGE_MODE developer;

    include /var/www/html/local.magento2.com/public_html/nginx.conf.sample;    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }
    error_log /var/log/nginx/default.error.log;
}
```

###<span id="相关文档">4. 相关文档</span>###
[http://dockone.io/article/834](http://dockone.io/article/834)
[Docker不能识别外部软链接](https://www.v2ex.com/amp/t/381563)
[https://yq.aliyun.com/ask/32406/?spm=5176.100241.0.0.yJlajg](https://yq.aliyun.com/ask/32406/?spm=5176.100241.0.0.yJlajg)
[ADD和COPY](http://cloud.51cto.com/art/201411/457338.htm)
[已非root的方式启动容器](https://stackoverflow.com/questions/29891010/how-to-run-docker-image-as-a-non-root-user)
[Docker下root与非root启动容器内应用](http://blog.csdn.net/yygydjkthh/article/details/47694929)
[https://docs.openshift.com/online/getting_started/beyond_the_basics.html#getting-started-beyond-the-basics](https://docs.openshift.com/online/getting_started/beyond_the_basics.html#getting-started-beyond-the-basics)