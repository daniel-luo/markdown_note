##Magento 1.x 用户登陆后返回执之前页面##

**注意这个代码是和后台配置一块儿使用**
`System --> Configuration --> Customers --> Customer Configuration --> Login Options`
`System --> Configuration --> Checkout --> Checkout Options --> Allow Guest Checkout`
```php
// allow guest checkout
if (Mage::helper('checkout')->isAllowedGuestCheckout($this->getOnepage()->getQuote()) == false) {
    if (!Mage::getSingleton('customer/session')->isLoggedIn()) {
            if (Mage::getStoreConfigFlag(Mage_Customer_Helper_Data::XML_PATH_CUSTOMER_STARTUP_REDIRECT_TO_DASHBOARD)) {
                Mage::getSingleton('customer/session')->setBeforeAuthUrl(Mage::getBaseUrl());
            } else {
                Mage::getSingleton('customer/session')->setBeforeAuthUrl($this->_getRefererUrl());
            }

            return $this->_redirect('customer/account/login');
    }
}
```