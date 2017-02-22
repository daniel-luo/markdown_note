**Author:**[Danel luo](luo3555mail@gmail.com)

###一，编译安装Mysql###
1. 安装c++ 编译库
```shell
sudo apt-get -y install cmake automake autoconf libtool gcc g++ bison
```

2. 安装 Mysql 依赖
```shell
sudo apt-get -y install libncurses5-dev
```

3. 创建目录，我们的php和mysql将会安装在这个目录下
```shell
sudo mkdir -p /opt/data/mysql/MYSQL_VERSION_NUMBER
sudo mkdir -p /opt/data/php/PHP_VERSION_NUMBER
```
**注意：**`MYSQL_VERSION_NUMBER`和`PHP_VERSION_NUMBER`需要替换为你自己对应的版本号

4. 添加mysql 用户和用户组
```shell
sudo groupadd mysql 
sudo useradd -r -g mysql mysql
```

5. 解压 mysql 源码包到当前目录，加入我们把解压到Download目录下，Ctrl + l复制当前文件夹路径
```shell
/home/userName/Downloads/mysql-5.6.26
```
这里的 `mysql` 版本是 `5.6.26` 所以 `MYSQL_VERSION_NUMBER` 就应该是 `5.6.26`

6. 命令行下进入 mysql 源码包目录，Ctrl + Alt + T 打开一个新的终端并进入mysql源码包目录
```shell
cd /home/userName/Downloads/mysql-5.6.26
```

7. 安装前配置mysql `确保以下是一条语句，所有参数和 configure 应该在同一行`
```shell
cmake . -DCMAKE_INSTALL_PREFIX=/opt/data/mysql/MYSQL_VERSION_NUMBER -DMYSQL_DATADIR=/opt/data/mysql/MYSQL_VERSION_NUMBER/data -DDEFAULT_COLLATION=utf8_general_ci -DDEFAULT_CHARSET=utf8 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_PERFSCHEMA_STORAGE_ENGINE=1
```

8. 编译Mysql
```shell
make
```

9. 编译完成后测试
```shell
make test
```

10. 测试通过后安装
```shell
sudo make install
```

11. 安装完成后进入安装mysql的根目录
```shell
cd /opt/data/mysql/MYSQL_VERSION_NUMBER
```
**注意：**`MYSQL_VERSION_NUMBER`和`PHP_VERSION_NUMBER`需要替换为你自己对应的版本号

12. 修改权限并初始化
```shell
sudo chown -R mysql .
sudo chgrp -R mysql .
sudo scripts/mysql_install_db --user=mysql
```
**有可能报错：**
```shell
FATAL ERROR: Neither host 'ann-Lenovo-Erazer-Z41-70' nor 'localhost' could be looked up with 
/usr/bin/resolveip 
Please configure the 'hostname' command to return a correct 
hostname. 
If you want to solve this at a later stage, restart this script 
with the --force option
```
执行下面的提示后再执行一下语句，`这一步是在初始化数据库很重要，如果没做这一步之后将无法启动数据库`
```shell
sudo scripts/mysql_install_db --user=mysql 
```
如果这一步**执行报错**可以尝试移除 `/etc/mysql/my.cnf`,这里我们不移除它而是将他的名字改变为`bak`
```shell
sudo mv /etc/mysql/my.cnf /etc/mysql/my.cnf.bak
```
数据库初始化以后执行一下语句修改文件夹权限
```shell
sudo chown -R root .
sudo chown -R mysql data
```

13. 安全模式启动Mysql
```shell
sudo bin/mysqld_safe --user=mysql & 
```
直接执行以下命令
根据提示初**始化密码**,提示类似以下命令行
```shell
sudo bin/mysqladmin -u root password '12345abc'
sudo bin/mysqladmin -u root -h localhost password '12345abc'
```

14. 添加Mysql到开机启动
打开一个新的终端Ctrl + Alt + T
```shell
sudo cp /opt/data/mysql/MYSQL_VERSION_NUMBER/support-files/mysql.server /etc/init.d/mysqld
sudo update-rc.d mysqld defaults 99
```
**注意：**这里的`MYSQL_VERSION_NUMBER`需要替换为你对应的版本号

15. 关闭所有终端，打开一个新的终端测试Mysql是否加入服务
Ctrl + Alt + T
```shell
sudo service mysqld start
sudo service mysqld stop
```

16. Mysql 安装完成后重启电脑
查看 mysql 是否启动,在终端中输入一下命令看能否进入到 Mysql
```shell
/opt/data/mysql/MYSQL_VERSION_NUMBER/bin/mysql
```
**注意：**这里的`MYSQL_VERSION_NUMBER`需要替换为你对应的版本号



###二，编译安装PHP###
1. 下载PHP源码包
[http://www.php.net](http://www.php.net)

2. 安装PHP依赖 Ctrl +Alt + T
```shell
sudo apt-get -y install libfreetype6-dev 
sudo apt-get -y install libxpm-dev 
sudo apt-get -y install libjpeg-dev 
sudo apt-get -y install libcurl3-openssl-dev 
sudo apt-get -y install libxml2-dev 
sudo apt-get -y install libicu-dev 
sudo apt-get -y install libmcrypt-dev 
sudo apt-get -y install libxslt1-dev 
sudo apt-get -y install zlib1g-dev 
sudo apt-get -y install libxml2-dev 
sudo apt-get -y install libxslt1-dev
```

3. 进入解压后的PHP文件夹
```shell
cd /home/userName/Downloads/php-5.6.13
```

4. 配置PHP参数 `确保以下是一条语句，所有参数和 configure 应该在同一行`
```shell
./configure --prefix=/opt/data/php/PHP_VERSION_NUMBER  --with-libxml-dir --with-openssl --with-zlib --with-curl --with-gd --with-jpeg-dir --with-png-dir --with-zlib-dir --with-freetype-dir --with-xpm-dir --with-mhash --with-pear  --with-mcrypt --with-icu-dir=DEFAULT --with-xsl  --with-mysql=/opt/data/mysql/use --with-mysqli=/opt/data/mysql/use/bin/mysql_config --with-pdo-mysql=/opt/data/mysql/use --enable-fpm --enable-soap --enable-zip --enable-pcntl --enable-ftp -enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-bcmath --enable-ctype --enable-exif --enable-ftp --enable-mbstring --enable-sockets --enable-wddx --enable-soap --enable-gd-native-ttf --enable-intl
```
**注意:**如果`congifure`报错找不到目录`/opt/data/mysql/use`,需要手动建立软连接：`sudo ln -s /opt/data/mysql/MYSQL_VERSION_NUMBER /opt/data/mysql/use`,完成后再次`configure`。这里的`MYSQL_VERSION_NUMBER`需要替换为你对应的版本号

5. 配置完成后编译并安装
```shell
make
make test
sudo make install
```

6. 测试PHP是否安装成功
```shell
cd /opt/data/php/PHP_VERSION_NUMBER/bin
./php -v
```
**注意：**这里的`PHP_VERSION_NUMBER`需要替换为你对应的版本号


###三，将MySQL和PHP加入环境变量###
1. 创建MySQL和PHP的软连接（为了以后能方便的切换版本）
```shell
sudo ln -s /opt/data/mysql/MYSQL_VERSION_NUMBER /opt/data/mysql/use
sudo ln -s /opt/data/php/PHP_VERSION_NUMBER /opt/data/php/use
```
**注意：**这里的`PHP_VERSION_NUMBER`和`MYSQL_VERSION_NUMBER`需要替换为你对应的版本号

2. 创建自定义环境变量文件，在自己的home目录下创建一个叫`.evn.bash`的空白文件
```shell
cd ~
gedit .env.bash
```
贴入以下代码后到`.env.bash`保存关闭
```shell
#!bash 
# Add custom evn path 
PHP_PATH=/opt/data/php/use/bin 
MYSQL_PATH=/opt/data/mysql/use/bin 
export PATH=$PHP_PATH:$MYSQL_PATH:$PATH
```
回到终端继续
```shell
gedit ~/.bashrc
```
在最末尾加入
```shell
source ~/.env.bash
```
保存关闭
回到终端输入一下命令
```shell
source ~/.bashrc
```
回车后输入
```shell
mysql
```
能进入则说明mysql已经成功加入到环境变量
```shell
php -v
```
有输出则说明已经成功加入到环境变量


###四，安装Apache###
1. 安装Ubuntu自带apache
```shell
sudo apt-get install apache2
```

2. 安装fcgid
```shell
sudo apt-get install libapache2-mod-fcgid
```

3. 配置fcgi
```shell
sudo cp /etc/apache2/mods-available/fcgid.conf /etc/apache2/mods-available/fcgid.conf.bak
sudo gedit /etc/apache2/mods-available/fcgid.conf
```
备份一份然后替换为以下内容（自带的默认配置参数太小一些PHP框架可能会跑死）
```text
<IfModule mod_fcgid.c>
  FcgidConnectTimeout 1200
    # Context - server config
    FcgidMaxProcesses 20
    # Otherwise php output shall be buffered
    FcgidOutputBufferSize 1
    FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 20
    FcgidMaxRequestsPerProcess 20
    FcgidIOTimeout  1200
    FcgidIdleTimeout  1200
    MaxRequestLen 15728640

  <IfModule mod_mime.c>
    AddHandler fcgid-script .fcgi
  </IfModule>
</IfModule>
```

4. 启用fcgi模块
```shell
sudo a2enmod fcgid
```

5. 修改默认虚拟机文件（修改的目的是让这个虚拟主机支持PHP）
```shell
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf.bak
sudo gedit /etc/apache2/sites-available/000-default.conf
```
替换为一下内容
```language
<VirtualHost *:80> 
    # The ServerName directive sets the request scheme, hostname and port that 
    # the server uses to identify itself. This is used when creating 
    # redirection URLs. In the context of virtual hosts, the ServerName 
    # specifies what hostname must appear in the request's Host: header to 
    # match this virtual host. For the default virtual host (this file) this 
    # value is not decisive as it is used as a last resort host regardless. 
    # However, you must set it for any further virtual host explicitly. 
    #ServerName www.example.com 
    ServerAdmin webmaster@localhost 
    DocumentRoot /var/www/html 

    # Available loglevels: trace8, ..., trace1, debug, info, notice, warn, 
    # error, crit, alert, emerg. 
    # It is also possible to configure the loglevel for particular 
    # modules, e.g. 
    #LogLevel info ssl:warn 

    <IfModule mod_fcgid.c> 
        ScriptAlias /fcgi-bin/ "/opt/data/php/use/bin" 
        <Directory /var/www/html/> 
            Options +ExecCGI 
            AllowOverride All 
            AddHandler fcgid-script .php 
            FCGIWrapper /opt/data/php/use/bin/php-cgi .php 
            Order allow,deny 
            Allow from all 
            Require all granted 
        </Directory> 
    </IfModule> 

    ErrorLog ${APACHE_LOG_DIR}/error.log 
    CustomLog ${APACHE_LOG_DIR}/access.log combined 

    # For most configuration files from conf-available/, which are 
    # enabled or disabled at a global level, it is possible to 
    # include a line for only one particular virtual host. For example the 
    # following line enables the CGI configuration for this host only 
    # after it has been globally disabled with "a2disconf". 
    #Include conf-available/serve-cgi-bin.conf 
</VirtualHost> 
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

6. 启用rewrite模块
```shell
sudo a2enmod rewrite
```

7. 应用新虚拟主机配置
```shell
sudo a2dissite 000-default
sudo a2ensite 000-default
sudo service apache2 restart
```

8.测试PHP
```shell
sudo gedit /var/www/html/phpinfo.php
```
加入
```php
<?php phpinfo() ?>
```

9. 测试，如果配置成功应该可以看到phpinfo的信息
[http://localhost/phpinfo.php](http://localhost/phpinfo.php)


###五，拓展###
为了方便以后数据的备份，我们把我们所有的配置文件和数据都放到 `/opt/data`下,这样以后备份只用备份`/opt`下的`data`目录数据库配置文件和代码就一次性备份了

1. 创建vhost配置文件夹用于存放所有虚拟主机配置
```shell
sudo mkdir /opt/data/apache
sudo mv /etc/apache2/sites-available /opt/data/apache/vhost
```
创建软连接
```shell
sudo ln -s /opt/data/apache/vhost /etc/apache2/sites-available
sudo ln -s /opt/data/apache/vhost /home/{userNameDir}/vhost
```
改变权限
```shell
sudo chown -R yourUserName:yourUserName /opt/data/apache/vhost
```
这样以后可以直接修改虚拟主机而不用每次都`sudo`了

2. 移动Apache网站根目录到 `/opt/data/html`
```shell
sudo mv /var/www/html /opt/data/html
```
添加软连接
```shell
sudo ln -s /opt/data/html /var/www/html
sudo ln -s /opt/data/html /home/{userNameDir}/www
```
修改权限
```shell
sudo chown -R  yourUserName:yourUserName /opt/data/html
```
3. 移动hosts文件
```shell
sudo cp /etc/hosts /opt/data/hosts
sudo chown -R yourUserName:yourUserName /opt/data/hosts
```
软连接到你的home目录
```shell
sudo ln -s /opt/data/hosts /home/{userNameDir}/hosts
```
替换系统原有的hosts文件
```shell
sudo mv /etc/hosts /etc/hosts.bak
sudo ln -s /opt/data/hosts /etc/hosts
```
这时你的home根目录(~表示你的home根目录)下会多出
 - ~/hosts　　　本地hosts文件
 - ~/vhost　　　虚拟主机配置文件(注意:这里配置文件里的根目录应该是/var/www/html开头而不是/opt/data/html)
 - ~/www           网站代码
这三个文件夹
4. www文件夹结构
www
|-php-version-number                 这个文件夹名是php版本号
|  |-local.test.com                         这个是文件夹名是网站域名
|  |  |-public_html                         网站代码放这个文件夹，同时git初始化也应该实在这个目录
|  |
|
|-php-version-number
|  |-local.test1.com
|  |  |-public_html
|
|-index.php
`Index.php`内容这段代码可以自动列出当前使用PHP版本和所有站点域名和后台链接
```php
<?php 
define('ROOT_DIR', $_SERVER["DOCUMENT_ROOT"]);
define('DS', DIRECTORY_SEPARATOR);
function dirList($path) 
{ 
    $dirResorce = dir($path); 
    $limit = ['.', '..', 'index.php']; 

    $list = []; 
    while (false !== ($entry = $dirResorce->read())) { 
        if (!in_array($entry, $limit)) { 
            $list[] = $entry; 
        } 
    } 

    return $list; 
} 
function renderWebsiteLinks($dirNames, $currentDir = null) 
{ 
    $html = ''; 

    if (empty($currentDir)) { 
        $currentDir = ROOT_DIR; 
    } 

    foreach($dirNames as $dirName) { 
        $dir = $currentDir . DS . $dirName; 
        if (preg_match("/.*\.com$/", $dir) && is_dir($dir)) { 
            $html .= '<li><a href="http://' . $dirName . '" target="_blank">' . $dirName . '[<a href="http://' . $dirName . '/admin" target="_blank">admin</a>]</li>'; 
        } else { 
            if (is_dir($dirName)) { 
                $html .= '<li><h4><a href="http://localhost/' . $dirName . '"</a>' . $dirName . '</h4></li>'; 
                $html .= '<li>' . renderWebsiteLinks(dirList($dir), $dir) . '</li>'; 
            } 
        } 
    } 

    return '<ul>' . $html . '</ul>'; 
} 
echo renderWebsiteLinks(dirList(ROOT_DIR)); 
echo '<hr/>'; 
phpinfo() 
?>
```
5. 切换默认MySQL或PHP版本
修改 `/opt/data/mysql/use` 和 `/opt/data/php/use` 软连接对应的文件夹即可
例如修改php为7.0.0版本
```shell
sudo rm -fr /opt/data/php/use
sudo ln -s /opt/data/php/7.0.0 /opt/data/php/use
```
这是在终端中输入
```shell
php -v
```
你会发现php版本已经切换为7.0了


###附录:###
编译PHP时可能遇到的错误以及处理方法
错误一： 
`configure: error: xml2-config not found. Please check your libxml2 installation. `
而我已经安装过了libxml2,但是还是有这个提示： 
解决办法：
```shell 
sudo apt-get -y install libxml2-dev 
```

错误二： 
`configure: error: Please reinstall the BZip2 distribution `
而我也已经安装了bzip2，网上找到得解决方案都是需要安装bzip2-dev，可是11.10里面没有这个库。 
解决办法：在网上找到bzip2-1.0.5.tar.gz，解压，直接`make ,sudo make install`.(我使用的该源来自于[http://ishare.iask.sina.com.cn/f/9769001.html](http://ishare.iask.sina.com.cn/f/9769001.html)) 

错误三： 
`configure: error: Please reinstall the libcurl distribution -easy.h should be in /include/curl/ `
解决办法： 
```shell
sudo apt-get install libcurl4-gnutls-dev 
```

错误四： 
`configure: error: jpeglib.h not found. `
解决办法： 
```shell
sudo apt-get install libjpeg-dev 
```

错误五： 
`configure: error: png.h not found. `
解决办法： 
```shell
sudo apt-get install libpng-dev 
```

错误六： 
`configure: error: libXpm.(a|so) not found. `
解决办法： 
```shell
sudo apt-get install libxpm-dev 
```

错误七： 
`configure: error: freetype.h not found. `
解决办法： 
```shell
sudo apt-get install libfreetype6-dev 
```

错误八： 
`configure: error: Your t1lib distribution is not installed correctly. Please reinstall it. `
解决办法： 
```shell
sudo apt-get install libt1-dev 
```

错误九： 
`configure: error: mcrypt.h not found. Please reinstall libmcrypt. `
解决办法： 
```shell
sudo apt-get install libmcrypt-dev 
```

错误十： 
`configure: error: Cannot find MySQL header files under yes. `
`Note that the MySQL client library is not bundled anymore! `
解决办法： 
```shell
sudo apt-get install libmysql++-dev 
```

错误十一： 
`configure: error: xslt-config not found. Please reinstall the libxslt >= 1.1.0 distribution `
解决办法： 
```shell
sudo apt-get install libxslt1-dev 
```
可见PHP源码安装之前需要先安装这些依赖，详细可见[http://forum.ubuntu.org.cn/viewtopic.php?f=88&t=231159](http://forum.ubuntu.org.cn/viewtopic.php?f=88&t=231159)