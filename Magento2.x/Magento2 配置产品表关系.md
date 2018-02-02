##Magento2 配置产品表关系##
`catalog_product_super_attribute` 存储当前产品需要配置的属性 `product_super_attribute_id` 为自动生成
`catalog_product_relation` 这个配置产品的子产品有哪些
`catalog_product_super_link` 这个配置产品的子产品有哪些

如果导入的子产品不出来，看一下表 `catalog_product_super_attribute` 看看是不是子产品属性不齐