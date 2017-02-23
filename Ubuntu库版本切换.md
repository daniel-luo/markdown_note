##Ubuntu 切换php版本##
```shell
update-alternatives --config php
```
```shell
sudo add-apt-repository ppa:ondrej/php
```

[切换自定源](http://atell.iteye.com/blog/1169556)

```shell
/home/wuekzhu/download/jdk1.5.0_22
/home/wuekzhu/download/jdk1.6.0_23
```
现在，我要使用update-alternatives将系统的默认java环境变成jdk1.6

首先使用 `update-alternatives --config java`，可以看到当前当前是使用 openjdk 的，
```shell
sudo update-alternatives  --install  /usr/bin/java java /home/wuekzhu/download/jdk1.6.0_23/bin/java   1888
```

切换
```shell
$ update-alternatives --config java
有 2 个候选项可用于替换 java (提供 /usr/bin/java)。

  选择       路径                                       优先级  状态
------------------------------------------------------------
  0            /usr/lib/jvm/java-6-openjdk/jre/bin/java      1061      自动模式
* 1            /home/wuekzhu/download/jdk1.6.0_23/bin/java   1         手动模式
  2            /usr/lib/jvm/java-6-openjdk/jre/bin/java      1061      手动模式
```