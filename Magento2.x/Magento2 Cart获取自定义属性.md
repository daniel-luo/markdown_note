##Magento2 Cart获取自定义属性
There is no necessity to change any PHP code for doing this.

You just need to create `{MODULE_NAME}/etc/catalog_attributes.xml` with such content:
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Catalog:etc/catalog_attributes.xsd">
    <group name="quote_item">
        <attribute name="sample_attr"/>
    </group>
</config>
```