#Modify custom option of product in cart

In the code above, `$_item` refers to an instance of `Mage_Sales_Model_Quote_Item`, so you can alter the buyRequest with the code below:

```php
$buyRequest = $_item->getOptionByCode('info_buyRequest');
$buyRequestArr = unserialize($buyRequest->getValue());
$buyRequestArr['whatever_you_like'] = 'some_value';
$buyRequest->setValue(serialize($buyRequestArr))->save();
```

However I'd definitely not advise altering the buyRequest from a phtml file, if you want to add data in to the buyRequest or change existing data, you should do so via an event observer by listening for `checkout_cart_product_add_after`, which supplied you with the quote item, so you can set data on the buyRequest like so:

```php
public function checkoutCartProductAddAfter(Varien_Event_Observer $observer)
{
    $item = $observer->getQuoteItem();
    $buyRequestArr = array();
    if ($buyRequest = $item->getProduct()->getCustomOption('info_buyRequest')) {
        $buyRequestArr = unserialize($buyRequest->getValue());
    }
    $buyRequestArr['whatever_you_like'] = 'some_value';
    $buyRequest->setValue(serialize($buyRequestArr))->save();
}
```

**Event**
events.xml
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Event/etc/events.xsd">
    <event name="catalog_product_load_after">
        <observer name="set_additional_options" instance="Webkul\Demo\Observer\SetAdditionalOptions"/>
    </event>
</config>
```

```php
<?php
/**
 * Webkul Software.
 *
 * @category  Webkul
 * @package   Webkul_Demo
 * @author    Webkul
 * @copyright Copyright (c) 2010-2016 Webkul Software Private Limited (https://webkul.com)
 * @license   https://store.webkul.com/license.html
 */
namespace Webkul\Demo\Observer;
 
use Magento\Framework\Event\ObserverInterface;
use Magento\Framework\App\RequestInterface;
 
class SetAdditionalOptions implements ObserverInterface
{
    /**
     * @var RequestInterface
     */
    protected $_request;
     
    /**
     * @param RequestInterface $request
     */
    public function __construct(
        RequestInterface $request
    ) {
        $this->_request = $request;
    }
 
    /**
     * @param \Magento\Framework\Event\Observer $observer
     */
    public function execute(\Magento\Framework\Event\Observer $observer)
    {
        // Check and set information according to your need
        if ($this->_request->getFullActionName() == 'checkout_cart_add') { //checking when product is adding to cart
            $product = $observer->getProduct();
            $additionalOptions = [];
            $additionalOptions[] = array(
                'label' => "Some Label",
                'value' => "Your Information",
            );
            $observer->getProduct()->addCustomOption('additional_options', serialize($additionalOptions));
        }
    }
}
```


##Ducuments
[https://magento.stackexchange.com/questions/59263/modify-custom-option-of-product-in-cart](https://magento.stackexchange.com/questions/59263/modify-custom-option-of-product-in-cart)
[How To Set Additional Options In Cart Item – Magento2](https://webkul.com/blog/additional-options-cart-item-magento2/)

