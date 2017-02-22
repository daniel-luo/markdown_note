推荐u盘使用exfat格式，为什么呢？两个原因：
1、三大主流操作系统（Linux、Mac、Windows）都支持exfat格式。
2、exfat支持大于4G的文件。

在ubuntu下，由于版权的原因（据说），默认不支持exfat格式的u盘，不过可以很方便就能添加对exfat的支持：

1、对于ubuntu 14.04版本，直接运行下面的命令就可以了：

```shell
sudo apt-get install exfat-utils
```

安装完之后重启生效。

2、对于ubuntu 12.04 ~ 13.10的版本，分别：

```shell
sudo add-apt-repository ppa:relan/exfat
sudo apt-get update
sudo apt-get install exfat-utils fuse-exfat
```

同样，重启电脑生效。

3、对于ubuntu 12.04之前的版本，可能得多费点功夫，点击以下链接查看：在Ubuntu OS中格式化、挂载exFAT硬盘/U盘


##Documents##
[https://github.com/dorimanx/exfat-nofuse](https://github.com/dorimanx/exfat-nofuse)
[http://www.jb51.net/os/Ubuntu/275158.html](http://www.jb51.net/os/Ubuntu/275158.html) 