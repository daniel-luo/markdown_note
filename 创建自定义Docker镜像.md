##创建 Docker镜像##
**Dockerfile**
`Dockerfile` 是为快速构建 `Docker Image` 而设计的，当你使用 `docker　build` 命令的时候，`docker` 会读取当前目录下的命名为 `Dockerfile` (首字母大写)的纯文本文件并执行里面的指令构建出一个 `docker image`。这种方法会更加自动化，更加方便快捷，而且功能也更强。

```Dockerfile
# Install Ubuntu
FROM ubuntu:16.04
MAINTAINER daniel.luo luo3555mail@sina.com
# update docker source
RUN apt-get update
# install supervisor and apache2
RUN apt-get install -y supervisor apache2
# config supervisor
RUN printf "[supervisord]\n\
numprocs=1\n\
autostart=true\n\
autorestart=true\n\
stdout_logfile=/var/log/supervisor/httpd.log\n\
stderr_logfile=/var/log/supervisor/httpd.log\n\
[program:apache]\n\
command=apachectl -D FOREGROUND\n" >> /etc/supervisord.conf

# add a bash for keep docker keep run, if not when you run docker you will get exist(0)
# run.sh content
##############################
# #!/bin/bash
# exec supervisord -n
##############################
ADD run.sh /run.sh
RUN chmod 755 /*.sh

# open container port
EXPOSE 80
# when run image default quto run command
CMD ["/run.sh"]

# build docker
#sudo docker build -t custom_tag_name:customer_version_number .
# enter docker
#sudo docker exec -i -t container_name /bin/bash
```

**Build Image**
```shell
#sudo docker build -t custom_tag_name:customer_version_number .
sudo docker build -t ubuntu:16.04 .
```
**Run Image**
```shell
sudo docker run -d containerID
```
**Enter Docker**
```shell
sudo docker exec -i -t your_container_name /bin/bash
```

**run.sh**
```shell
#!/bin/bash
exec supervisord -n
```

###Documents###
[http://www.infoq.com/cn/articles/docker-core-technology-preview](http://www.infoq.com/cn/articles/docker-core-technology-preview)
[http://www.runoob.com/docker/docker-exec-command.html](http://www.runoob.com/docker/docker-exec-command.html)
[http://www.cnblogs.com/linjiqin/p/3625609.html](http://www.cnblogs.com/linjiqin/p/3625609.html)
[http://www.docker.org.cn/](http://www.docker.org.cn/)
[http://cloud.51cto.com/art/201411/457333.htm](http://cloud.51cto.com/art/201411/457333.htm)
[http://www.cnblogs.com/elnino/p/3899136.html](http://www.cnblogs.com/elnino/p/3899136.html)