##Docker##
When you look at your old format:

```shell
sudo docker build -t mysshdimage .
```

the . tells docker where your build context (files) are for the build. You still need to give docker that information when you use the -f option. So, try:

```shell
sudo docker build -t mysshdimage -f sshd_dockerfile .
```


##Run##
```shell
将 docker　中 /var/www/mage2 这个点挂在到 mage2_san
## docker run -v /var/www/mage2 --name mage2_san busybox true
启动apache和外部物理机80端口绑定，8888端口绑定，链接上mysql 文件卷标为 mage2_san，启动的是景象名为magento版本为2.1.0的这个景象
## docker run --name mage2_apache -d -p 80:80 -p 8888:8888 --link mysql --volumes-from mage2_san magento:2.1.0
```

**Docker 查看有那些镜像**
```shell
sudo docker images
```
**Result**
tag 是你自己创建的版本号
```shell
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
magento2            latest              8a44ae9d391c        2 days ago          584 MB
mysql               5.6                 376d97cce39c        2 days ago          783 MB
busybox             latest              7968321274dc        7 weeks ago         1.11 MB
centos              6.7                 ab44245321a8        6 months ago        191 MB

```

##进入Docker的容器##
**查看有那些镜像在运**
`$ sudo docker ps`
**进入镜像 magento2 并运行 /bin/bash**
`$ sudo docker exec -it magento2 /bin/bash`


```shell
# 为 MySql 创建一个 busybox 名为 mysql_container 并后台运行
sudo docker run -v /var/lib/mysql --name mysql_container busybox true
# 运行一个 mysql版本为5.6 的镜像，数据挂载点为刚创建的 mysql_container
sudo docker run --name mysql_server -d -p 3306:3306 --volumes-from mysql_container mysql:5.6

#为 /var/www/mage2 这个文件夹创建一个数据容器 magento2_contener
sudo docker run -v /var/www/mage2 --name magento2_contener busybox true
＃启用 magento2:latest 这个镜像， -d 在后台运行， -p端口绑定, 与这个镜像一起工作的还有 --link mysql_server这个镜像
＃mysql_server 是启动前一个镜像时，用户给那个镜像取的名字，--volumes-from　当前这个镜像的数据存储点
sudo docker run --name magneto2_apache2 -d -p 80:80 -p 8888:8888 --link mysql_server --volumes-from magento2_contener magento2:latest
```



##Documents##
[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04#step-1-%E2%80%94-installing-docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04#step-1-%E2%80%94-installing-docker)
[https://github.com/docker/docker/issues/10910](https://github.com/docker/docker/issues/10910)
[http://www.ifanr.com/app/584716](http://www.ifanr.com/app/584716)
[http://blog.csdn.net/u010397369/article/details/41045251](http://blog.csdn.net/u010397369/article/details/41045251)
[https://help.getsync.com/hc/en-us/articles/206178924](https://help.getsync.com/hc/en-us/articles/206178924)
[http://yaxin-cn.github.io/Docker/how-to-delete-a-docker-image.html](http://yaxin-cn.github.io/Docker/how-to-delete-a-docker-image.html)
[http://blog.csdn.net/wsscy2004/article/details/25878363](http://blog.csdn.net/wsscy2004/article/details/25878363)
[Back up and restore container data](https://getcarina.com/docs/tutorials/backup-restore-data/)
[docker-import-mysql](https://hub.docker.com/r/kloplop321/docker-import-mysql/)
[Docker Images Manager](https://kitematic.com/)
[https://github.com/docker/kitematic/issues/49](https://github.com/docker/kitematic/issues/49)
[https://github.com/docker/kitematic/releases](https://github.com/docker/kitematic/releases)
[https://blog.confirm.ch/backup-mysql-mariadb-docker-container/](https://blog.confirm.ch/backup-mysql-mariadb-docker-container/)