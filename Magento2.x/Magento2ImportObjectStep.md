##Magento2 Import Exist Object Step##

1. 导入数据库
2. 修改域名，执行一下 SQL
```sql
UPDATE core_config_data set value='http://local.hollywood.com/' where path='web/unsecure/base_url' or path='web/secure/base_url';
```
3. Check
```sql
select value from core_config_data  where (path='web/unsecure/base_url' or path='web/secure/base_url');
```
4. 修改 `app/etc/env.php` 文件
5. 确保 `app/etc/config.php` 文件存在
6. 安装依赖,在网站根目录下执行 `composer install`

​###composer install Auth###

当提示如下信息的时候你需要去 Magento 官网登陆获取你的授权 key:

```shell
  - Installing magento/framework (100.1.0)
    Authentication required (repo.magento.com):
```

1. 登陆 Magento 官网
2. 点击顶部 Connect tab
3. 点击左边栏里的 Secure Keys
4. 你可以看到一个叫 Magento2 的 Secure Keys。这里 Public key 作为 Name, Private key 作为密码

####auto.json 自动验证####

如果不想每次都输入验证可以在根目录 `composer.json` 配置自动验证文件 `auto.json`,验证内容如下:
```json
{
    "http-basic": {
        "repo.magento.com": {
            "username":"<your public key>",
            "password":"<your private key>"
        }
    }
}
```

*****

相关资料:
[自动验证](http://magento.stackexchange.com/questions/90983/how-to-use-the-new-repo-magento-com)
[Magento部署,获取验证用户名和密码](http://devdocs.magento.com/guides/v2.1/install-gde/prereq/connect-auth.html)