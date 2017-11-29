##Magento2 自定义 API
文件夹结构
```text
Tenf_Test
|--etc
|   |--module.xml
|   |--webapi.xml
|
|--registration.php
|
|--Model
|  |--Hello.php
|
|--Api
|  |--HelloInterface.php
|
|
|
|
|
|
```

添加模块声明文件 **etc/module.xml**

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
配置API **etc/webapi.xml**
```xml
<?xml version="1.0"?>
<routes xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Webapi:etc/webapi.xsd">
    <route url="/V1/test/hello/:name" method="GET">
        <service class="Tenf\Test\Api\HelloInterface" method="name"/>
        <resources>
            <resource ref="anonymous"/>
        </resources>
    </route>
</routes>
```
- - -
定义接口 **etc/di.xml**
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <preference for="Tenf\Test\Api\HelloInterface"
                type="Tenf\Test\Model\Hello" />
</config>
```
- - -
创建接口文件 **Api/HelloInterface.php**
```php
namespace Tenf\Test\Api;

interface HelloInterface
{
    /**
     * Returns greeting message to user
     *
     * @api
     * @param string $name Users name.
     * @return string Greeting message with users name.
     */
    public function name($name); // 一个参数

    /**
     * Base product id and type id get products
     *
     * @api
     * @param string $productId Product Id.
     * @param string $typeId
     * @return string Greeting message Product.
     */
    public function products($productId, $typeId); // 两个参数，且类型是 string
    // 如果不知道参数类型可以写成 @params max
}
```
**注意:**
Magento 会根据接口中方法的doc文档检查参数和参数类型，如果参数或类型不匹配会抛出异常
```log
Class not exist
```


- - -
创建模型 **Model/Hello.php**
```php
namespace Tenf\Test\Model;
use Tenf\Test\Api\HelloInterface;

class Hello implements HelloInterface
{
    /**
     * Returns greeting message to user
     *
     * @api
     * @param string $name Users name.
     * @return string Greeting message with users name.
     */
    public function name($name) {
        return "Hello, " . $name;
    }
}
```

- - -
添加ACL **etc/acl.xml**
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Acl/etc/acl.xsd">
    <acl>
        <resources>
            <resource id="Magento_Backend::admin">
                <resource id="Tenf_Test::hello" title="Hello" translate="title" sortOrder="110" />
            </resource>
        </resources>
    </acl>
</config>
```

安装
```shell
php bin/magento setup:upgrade
```
- - -
测试
`http://local.magento2.com/rest/V1/test/hello/Daniel`


##Documents
[http://inchoo.net/magento/api-magento/magento-2-custom-api/](http://inchoo.net/magento/api-magento/magento-2-custom-api/)
[Passing Array in REST Api Magento2 (Post Method)](http://devdocs.magento.com/guides/v2.1/extension-dev-guide/service-contracts/service-to-web-service.html)