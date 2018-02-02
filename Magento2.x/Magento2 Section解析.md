##Magento2 Section解析

1. **seciton.xml**中添加记录
```xml
<?xml version="1.0"?>
<!--
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Customer:etc/sections.xsd">
    <action name="test/process/observer">
        <section name="cart"/>
    </action>
</config>
```

2. **或者js**强制刷新
```javascript
require([
           'Magento_Customer/js/customer-data'
        ], function (customerData) {
            var sections = ['cart'];
            customerData.invalidate(sections);
            customerData.reload(sections, true);
        });
```
OR
```javascript
require('Magento_Customer/js/customer-data').reload(['cart'], false);
```
强制刷新`cart`这个`section`

##Section##
`vendor/magento/magento2-base/lib/web/mage/apply/scripts.js`会寻找页面上所有标记为`text/x-magento-init`的节点

---

以`vendor/magento/module-checkout/view/frontend/templates/cart/totals.phtml`为例
```html
<div id="cart-totals" class="cart-totals" data-bind="scope:'block-totals'">
    <!-- ko template: getTemplate() --><!-- /ko -->
    <script type="text/x-magento-init">
            {
                "#cart-totals": {
                    "Magento_Ui/js/core/app": <?php /* @escapeNotVerified */ echo $block->getJsLayout();?>
                }
            }
    </script>
</div>
```
`scripts.js`脚本会获取到`totals.phtml`里的`text/x-magento-init`然后对节点中定义的`#cart-totals`进行渲染

```json
{
    "#cart-totals": {
        "Magento_Ui/js/core/app": <?php /* @escapeNotVerified */ echo $block->getJsLayout();?>
    }
}
```
**#cart-totals**页面上有这个节点时，对当前节点进行渲染,也就是当前`text/x-magento-init`的位置
**Magento_Ui/js/core/app** 用这个`js`文件对以下配置进行渲染;当前`getJsLayout()`得到以下[配置信息](#config-json),以`dicount`为例，使用`Magento_SalesRule/js/view/cart/totals/discount`这个js对当前区域进行渲染，对应的初始化配置信息为`config:{title:'xxxxxx'}`,如果是自己写的渲染脚本可以尝试一下方法．`Magento_SalesRule/js/view/cart/totals/discount`对应`Magento_SalesRule`模块下`view/web/js/view/cart/totals/discount.js`
```json
{
	"#rend-node-id":{
    	"our render js":{ // 我们用于渲染的js
        	"config" : { // 该节点作为options传入渲染脚本 options.config
            	"title":"test"
            }
        }
    }
}
```
参考File:`vendor/magento/module-banner/view/frontend/templates/js/banner.phtml`.自定义的脚本中`uiComponent`是必须的默认使用它进行渲染的
以下代码待验证
`custom.js`
```javascript
define(
    [
        'uiComponent'
    ],
    function (Component) {
    	return Component.extend({
            defaults: {
                template: 'Magento_SalesRule/summary/discount' // 定义渲染要用的模板
            }
        });
    });
```
如果要在`custom.js`中对代码中数据进行持续监听需要添加`ko`，并监听
```javascript
define(
[
    'uiComponent',
    'ko'
],
function (Component,ko) {
    return Component.extend({
        defaults: {
            template: 'Magento_SalesRule/summary/discount' // 定义渲染要用的模板
        },
        customFunction: function() {
            // 持续监听页面上 window.checkoutConfig.totalsData 数据的变化
            var totalsData = window.checkoutConfig.totalsData;
            ko.observable(totalsData);
        }
    });
});
```

* * *

<span id="#config-json">config.json</span>
```json
{
    "components": {
        "block-totals": {
            "children": {
                "before_grandtotal": {
                    "children": {
                        "discount": {
                            "component": "Magento_SalesRule/js/view/cart/totals/discount",
                            "config": {
                                "title": "Discount"
                            }
                        },
                        "giftCardAccount": {
                            "component": "Magento_GiftCardAccount/js/view/cart/totals/gift-card-account",
                            "sortOrder": "30",
                            "config": {
                                "title": "Gift Card"
                            }
                        },
                        "gift-wrapping-order-level": {
                            "component": "Magento_GiftWrapping/js/view/checkout/cart/totals",
                            "config": {
                                "title": "Gift Wrapping for Order",
                                "level": "order"
                            }
                        },
                        "gift-wrapping-item-level": {
                            "component": "Magento_GiftWrapping/js/view/checkout/cart/totals",
                            "config": {
                                "title": "Gift Wrapping for Items",
                                "level": "item"
                            }
                        },
                        "printed-card": {
                            "component": "Magento_GiftWrapping/js/view/checkout/cart/totals",
                            "config": {
                                "title": "Printed Card",
                                "level": "printed-card"
                            }
                        },
                        "reward": {
                            "component": "Magento_Reward/js/view/cart/reward",
                            "config": {
                                "template": "Magento_Reward/cart/reward",
                                "title": "Reward points"
                            }
                        },
                        "dynamicprice": {
                            "config": {
                                "title": "Special Promotion Price Applied"
                            }
                        }
                    },
                    "component": "uiComponent",
                    "sortOrder": "30"
                },
                "subtotal": {
                    "component": "Magento_Tax/js/view/checkout/summary/subtotal",
                    "config": {
                        "title": "Subtotal",
                        "template": "Magento_Tax/checkout/summary/subtotal",
                        "excludingTaxMessage": "(Excl. Tax)",
                        "includingTaxMessage": "(Incl. Tax)"
                    },
                    "sortOrder": "10"
                },
                "shipping": {
                    "component": "Magento_Tax/js/view/checkout/cart/totals/shipping",
                    "config": {
                        "title": "Shipping",
                        "template": "Magento_Tax/checkout/cart/totals/shipping",
                        "excludingTaxMessage": "Excl. Tax",
                        "includingTaxMessage": "Incl. Tax"
                    },
                    "sortOrder": "30"
                },
                "grand-total": {
                    "component": "Magento_Tax/js/view/checkout/cart/totals/grand-total",
                    "config": {
                        "title": "Order Total",
                        "template": "Magento_Tax/checkout/cart/totals/grand-total",
                        "exclTaxLabel": "Order Total Excl. Tax",
                        "inclTaxLabel": "Order Total Incl. Tax"
                    },
                    "sortOrder": "100"
                },
                "customerbalance": {
                    "component": "Magento_CustomerBalance/js/view/cart/summary/customer-balance",
                    "config": {
                        "title": "Store Credit",
                        "template": "Magento_CustomerBalance/cart/summary/customer-balance"
                    },
                    "sortOrder": "95"
                },
                "tax": {
                    "component": "Magento_Tax/js/view/checkout/cart/totals/tax",
                    "config": {
                        "template": "Magento_Tax/checkout/cart/totals/tax",
                        "title": "Tax"
                    },
                    "sortOrder": "40"
                },
                "weee": {
                    "component": "Magento_Weee/js/view/cart/totals/weee",
                    "config": {
                        "title": "FPT"
                    },
                    "sortOrder": "35"
                },
                "custom-shippingmethod": {
                    "component": "Silk_CustomShippingMethod/js/view/checkout/cart/totals/shipping",
                    "sortOrder": "35",
                    "config": {
                        "template": "Silk_CustomShippingMethod/checkout/cart/totals/shipping"
                    }
                }
            },
            "component": "Magento_Checkout/js/view/cart/totals",
            "displayArea": "totals",
            "config": {
                "template": "Magento_Checkout/cart/totals"
            }
        }
    }
}
```



##Documents
[强制刷新section](https://magento.stackexchange.com/questions/100615/magento-2-how-can-refresh-minicart-cache-after-clear-cart-session-and-place-orde)
[刷新section](https://magento.stackexchange.com/questions/208223/how-to-force-reloading-of-a-customer-data-section)
[https://magento.stackexchange.com/questions/112948/magento-2-how-do-customer-sections-sections-xml-work](https://magento.stackexchange.com/questions/112948/magento-2-how-do-customer-sections-sections-xml-work)
[What are Magento Sections](https://alanstorm.com/understanding-the-limitations-of-sectionsxml/)
[了解section.xml限制和原理](https://alanstorm.com/understanding-the-limitations-of-sectionsxml/)
[https://magento.stackexchange.com/questions/103861/how-to-use-knockout-js-within-magento-2](https://magento.stackexchange.com/questions/103861/how-to-use-knockout-js-within-magento-2)
[https://magento.stackexchange.com/questions/112948/magento-2-how-do-customer-sections-sections-xml-work](https://magento.stackexchange.com/questions/112948/magento-2-how-do-customer-sections-sections-xml-work)
[https://magento.stackexchange.com/questions/103861/how-to-use-knockout-js-within-magento-2](https://magento.stackexchange.com/questions/103861/how-to-use-knockout-js-within-magento-2)
[Magento 2: UI Component Data Sources](https://alanstorm.com/magento-2-ui-component-data-sources/)