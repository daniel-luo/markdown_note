##Git多个远程仓库##
例如我有下面两个仓库:
```shell
git@bitbucket.net:fancive/project.git
git@github.com:fancive/curl.git
```

在项目路径下打开Git Bash
添加一个remote,这里是origin,也可以是别的名字

```shell
git remote add origin git@bitbucket.net:fancive/project.git
git remote set-url --add origin git@github.com:fancive/curl.git
```

如果有多个,按照上面这一个命令进行添加.
提交的时候输入:

```shell
git push origin --all
```

##Documents##
[http://blog.csdn.net/fancivez/article/details/51544354](http://blog.csdn.net/fancivez/article/details/51544354)