##Magento2 创建基础模块
文件夹结构
```text
Tenf_Test
|--etc
|   |--module.xml
|
|--registration.php
|
|
|
|
|
|
|
|
```

添加模块声明文件 **module.xml**

```xml
<?xml version="1.0"?>

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Tenf_Test" setup_version="1.0.0">
    </module>
</config>
```
- - -

注册模块 **registration.php**
```php
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'Tenf_Test',
    __DIR__
);
```
- - -
安装
```shell
php bin/magento setup:upgrade
```
- - -


##Documents
[http://inchoo.net/magento-2/how-to-create-a-basic-module-in-magento-2/](http://inchoo.net/magento-2/how-to-create-a-basic-module-in-magento-2/)