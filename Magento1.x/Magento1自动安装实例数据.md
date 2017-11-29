##Magento 1.x 自动安装实例数据##
1. 在 `data/xxx_setup` 文件夹为相应的版本创建初始化数据文件 `data-install-0.1.0.php`
2. 数据升级文件， 同样在 `data/xxx_setup` 文件夹下创建文件 `data-upgrade-0.1.0-0.1.1.php`

**文件夹结构**

```text
Tenf_Data
|-etc
|  |-config.xml
|
|-Model
|
|-sql
|  |-tenf_data_setup
|  |  |-mysql4-install-0.1.0.php
|     |-mysql4-upgrade-0.1.0-0.1.1.php
|
|-data
|  |-tenf_data_setup
|  |  |-data-install-0.1.0.php
|     |-data-upgrade-0.1.0-0.1.1.php
|
|
```



**安装文件**
`data-inatall-0.1.0.php`

```php
$tickets = array(
    array(
        'title' => 'Broken cart',
        'description' => 'Unable to add items to cart.',
    ),
    array(
        'title' => 'Login issues',
        'description' => 'Cannot login when using IE.',
    ),
);

foreach ($tickets as $ticket) {
    Mage::getModel('inchoo_dbscript/ticket')
        ->setData($ticket)
        ->save();
}
```

**升级文件**
`data-upgrade-0.1.0-0.1.1.php`

```php
$tickets = Mage::getModel('inchoo_dbscript/ticket')
                ->getCollection();

foreach ($tickets as $ticket) {
    $ticket->setCreatedAt(strftime('%Y-%m-%d %H:%M:%S', time()))
           ->save();
}
```

##Documents##
[http://inchoo.net/magento/magento-install-install-upgrade-data-and-data-upgrade-scripts/](http://inchoo.net/magento/magento-install-install-upgrade-data-and-data-upgrade-scripts/)