##Magento2 Shopping Cart Add Product Customer Attribute##
在自己的模块下创建爱你对应的 xml 文件例如：`{MODULE_NAME}/etc/catalog_attributes.xml`
加入以下内容:

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Catalog:etc/catalog_attributes.xsd">
    <group name="quote_item">
        <attribute name="customer_attribute_code"/>
    </group>
</config>
```

见: `vendor/magento/module-sales/etc/catalog_attributes.xml` **Line 9** `<group name="quote_item">`

**注意:** 添加后需要清空缓存，重新部署静态文件