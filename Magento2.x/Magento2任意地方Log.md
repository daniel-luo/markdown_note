##Magento2任意地方Log
```php
$writer = new \Zend\Log\Writer\Stream(BP . '/var/log/motion_api.log');
$logger = new \Zend\Log\Logger();
$logger->addWriter($writer);
$logger->info(serialize($data));
```