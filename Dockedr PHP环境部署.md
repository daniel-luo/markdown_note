##Dockedr PHP环境部署##
[1. 安装Docker](#安装Docker)
[2. Docker基本命令](#Docker基本命令)
[3. 搭建Nginx + PHP + Mysql环境](#搭建Nginx + PHP + Mysql环境)

***

###<span id="安装Docker">1. 安装Docker</span>###


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
FROM ubuntu:trusty

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

**supervisord.conf**
```text
[supervisord]
nodaemon=true

[program:php5.6-fpm]
command=/usr/sbin/php-fpm5.6

[program:php7.0-fpm]
command=/usr/sbin/php-fpm7.0

[program:nginx]
command=/usr/sbin/nginx
```
