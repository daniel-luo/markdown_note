##Magento2 加入日志记录##

**Code:**

```php
   /**
    * @var \Psr\Log\LoggerInterface
    */
    private $logger;

     /**
     * Add new dependency
     *
     * @return \Psr\Log\LoggerInterface
     *
     * @deprecated
     */
    private function getLogger()
    {
        if (!$this->logger instanceof \Psr\Log\LoggerInterface) {
            $this->logger = \Magento\Framework\App\ObjectManager::getInstance()->get(\Psr\Log\LoggerInterface::class);
        }
        return $this->logger;
    }

    // use
    // $this->getLogger()->critical($exception);
```


##Document##
[https://github.com/magento/magento2/commit/5c93580f892c7f7a4032ac03e6d5e9a69f01390f](https://github.com/magento/magento2/commit/5c93580f892c7f7a4032ac03e6d5e9a69f01390f)