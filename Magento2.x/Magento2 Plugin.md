##Magento2 Plugin
**文件结构**
```text
Tenf
|--Plugin
|  |--etc
|  |  |--registration.php
|  |  |--module.xml
|  |  |--di.xml
|  |
|  |--Plugin
|  |  |--Product.php
|  |
```

**1. etc/registration.php**
```php
/**
 * Copyright (c) 2018
 * @authors daniel (luo3555@qq.com)
 * @date    18-1-26 下午3:08
 * @version 0.1.0
 */
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'Tenf_Plugin',
    __DIR__
);
```

**2. etc/module.xml**
```xml
<?xml version="1.0"?>
<!--
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 * daniel luo (luo3555@qq.com)
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Tenf_Plugin" setup_version="1.0.0">
    </module>
</config>
```

**3. etc/di.xml**
```xml
<?xml version="1.0"?>
<!--
/**
 * Copyright © 2013-2017 Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Magento\Catalog\Model\Product">
        <plugin name="custome_plugin_name" type="Tenf\Plugin\Plugin\Product" sortOrder="1" disabled="false"/>
    </type>
</config>
```
**注意**
1. `<type name="Magento\Catalog\Model\Product">`表示该`Plugin`对`Magento\Catalog\Model\Product`这个类中的方法起作用
2. ` <plugin name="custome_plugin_name"` 这里的`name`为自定义的唯一标记
3. `type="Tenf\Plugin\Plugin\Product"`我们的`Plugin`的具体实现

**4. Plugin/Product.php**
```php
namespace Tenf\Plugin\Plugin;

class Product
{
  /**
   * 这里我们对获取 Special price 后进行操作
   */
    /**
     * @param \Magento\Catalog\Model\Product $product
     * @param $result
     * @return float
     */
    public function afterGetSpecialPrice(\Magento\Catalog\Model\Product $product, $result)
    {
    	// Here do what you want to do.
        return $result;
    }
}
```
**注意**
1. `Plugin`不用继承任何类
2. 在这个`Plugin`里我们对`getSpecialPrice`方法执行后，对其结果进行加工，因此方法名是`after+getSpecialPrice`
3. **`afterGetSpecialPrice`函数的第一个参数必须是我们在`di.xml`里定`\Magento\Catalog\Model\Product`类的实例**,`$result`为上一步`getSpecialPrice`返回的值

##Documents
[http://devdocs.magento.com/guides/v2.0/extension-dev-guide/plugins.html](http://devdocs.magento.com/guides/v2.0/extension-dev-guide/plugins.html)
[https://www.mageplaza.com/how-to-change-product-price-with-plugin-magento-2.html](https://www.mageplaza.com/how-to-change-product-price-with-plugin-magento-2.html)
[http://magento.stackexchange.com/questions/114128/magento-2-plugin-before-around-after-interaction](http://magento.stackexchange.com/questions/114128/magento-2-plugin-before-around-after-interaction)
[https://cyrillschumacher.com/magento2-list-of-all-dispatched-events/](https://cyrillschumacher.com/magento2-list-of-all-dispatched-events/)
[http://magento.stackexchange.com/questions/32957/how-to-instantiate-a-block-in-magento2](http://magento.stackexchange.com/questions/32957/how-to-instantiate-a-block-in-magento2)



