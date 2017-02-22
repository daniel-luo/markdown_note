##配置 SSL##

1. 安装
`sudo apt-get install openssl`

2. 创建 ssl 文件夹
3. `mkdir /etc/apache2/ssl`
4. 创建并签发证书
`cd /etc/apache2/ssl`
创建CA签名(不使用密码去除-des3选项)
`openssl genrsa -des3 -out server.key 1024`
这时候会提示输入密码，随便输入，之后会清除的
去除key文件口令的命令:
`openssl rsa -in server.key -out server.key`
创建CSR(Certificate Signing Request)
`openssl req -new -key server.key -out server.csr`
自己签发证书
`openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt`
5. 启用模块
`sudo a2enmod ssl`
`LoadModule ssl_module modules/mod_ssl.so`
6. 添加虚拟主机

```txt
<VirtualHost *:80>
    ServerAdmin support@tenfsoftware.com
    DocumentRoot "/opt/data/apache/use/htdocs/default/local.magento.com/public_html"
    ServerName local.magento.com

    # Add php support, through fcgid_module
    <IfModule fcgid_module>
        ScriptAlias /fcgi-bin/ "/opt/data/php/default/bin/"
        <Directory /opt/data/apache/use/htdocs/default/local.magento.com/public_html>
            Options +ExecCGI

            AllowOverride All
            AddHandler fcgid-script .php
            FCGIWrapper /opt/data/php/CGI/default.cgi .php
            Order allow,deny
            Allow from all
            Require all granted
        </Directory>
    </IfModule>

    ErrorLog "logs/local.magento.com-error_log"
    CustomLog "logs/local.magento.com-access_log" common
</VirtualHost>

<VirtualHost *:443>
    ServerName local.magento.com
    DocumentRoot "/opt/data/apache/use/htdocs/default/local.magento.com/public_html"

    # Add php support, through fcgid_module
    <IfModule fcgid_module>
        ScriptAlias /fcgi-bin/ "/opt/data/php/default/bin/"
        <Directory /opt/data/apache/use/htdocs/default/local.magento.com/public_html>
            Options +ExecCGI

            AllowOverride All
            AddHandler fcgid-script .php
            FCGIWrapper /opt/data/php/CGI/default.cgi .php
            Order allow,deny
            Allow from all
            Require all granted
        </Directory>
    </IfModule>

    SSLEngine On
    SSLOptions +StrictRequire
    SSLCertificateFile /opt/data/apache/use/conf/server.crt
    SSLCertificateKeyFile /opt/data/apache/use/conf/server.key
</VirtualHost>
```
