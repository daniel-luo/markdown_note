##Magento2 复写 Action##
###复写购物车更新###
Tenf
 |-Checkout
 |  |
 |  |-Controller
 |     |
 |     |-Cart
 |        |-UpdatePost.php
 |
 |-etc
 |  |-di.xml


1. 在 `di.xml` 中添加要复写的 **Action**

```xml
<?xml version="1.0"?>
<!--
/**
 * Copyright © 2015 Silksoftware. All rights reserved.
 */
-->

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">

     <preference for="Magento\Checkout\Controller\Cart\UpdatePost" type="Tenf\Checkout\Controller\Cart\UpdatePost" />

</config>
```

2. 在对应位置重写复写的 **Action**

```php
<?php
/**
 *
 * Copyright © 2016 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
namespace Tenf\Checkout\Controller\Cart;

class UpdatePost extends \Magento\Checkout\Controller\Cart\UpdatePost
{
    /**
     * Update customer's shopping cart
     *
     * @return void
     */
    protected function _updateShoppingCart()
    {
        // Your logic ...
    }
}


```