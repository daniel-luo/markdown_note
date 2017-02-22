##Magento2 获取绝对路径##

```php
// $reader @see Magento\Framework\Filesystem\Directory
$reader = Magento\Framework\App\ObjectManager::getInstance()->get('Magento\Framework\Filesystem')
                    ->getDirectoryRead('media');
$reader->getAbsolutePath();

```

##Documents##
[https://www.extensionsmall.com/blog/get-absolute-path-file-magento-2/](https://www.extensionsmall.com/blog/get-absolute-path-file-magento-2/)
[http://stackoverflow.com/questions/34872249/absolute-path-in-magento2](http://stackoverflow.com/questions/34872249/absolute-path-in-magento2)