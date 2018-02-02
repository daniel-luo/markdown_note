##Magento2 创建命令行工具##

文件及其结构

```text
Temp
 |
 |--Shell
 |   |
 |   |--registration.php
 |   |
 |   |--etc
 |   |   |--module.xml
 |   |   |--di.xml
 |   |
 |   |--Console
 |   |   |--Command
 |   |   |   |--Customer.php
 |
```

a. `app/code/Temp/Shell/etc/module.xml`

```xml
<?xml version="1.0"?>
<!--
/**
 * Copyright © 2016 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Temp_Shell" setup_version="1.0.0">
    </module>
</config>
```

b. `app/code/Temp/Shell/etc/di.xml`

```xml
<?xml version="1.0"?>
<!--
/**
 * Copyright © 2016 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Magento\Framework\Console\CommandList">
        <arguments>
            <argument name="commands" xsi:type="array">
                <item name="custome_shell" xsi:type="object">Temp\Shell\Console\Command\Customer</item>
            </argument>
        </arguments>
    </type>
</config>

```

c. `app/code/Temp/Shell/registration.php`

```php
<?php
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'Temp_Shell',
    __DIR__
);
```

d. `app/code/Temp/Shell/Console/Command/Customer.php`

```php
<?php
/**
 * Copyright © 2016 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
namespace Temp\Shell\Console\Command;
use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
/**
 * Class GreetingCommand
 */
class Customer extends Command
{
    protected function configure()
    {
        $this->setName('shell:customer')
            ->setDescription('Customer command');
        parent::configure();
    }


    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $_objManger = \Magento\Framework\App\ObjectManager::getInstance();
        $catetory = $_objManger->create('\Magento\Catalog\Model\Category');

        $collection = $catetory->getCollection();

        foreach ($collection as $item) {
            print_r($item->getData());
        }

        print_r($collection);

        echo PHP_EOL . "Finish!" . PHP_EOL;
    }
}
```

e. 安装更新

```php
php bin/magento setup:upgrade
```

##常见错误以及解决办法##
在处理问题前请确保环境为 `developer` 模式， 如果不是可能需要另编译才会生效

问题一
[http://magento.stackexchange.com/questions/96747/area-code-not-set-issue-in-custom-cli-commands-in-magento-2](http://magento.stackexchange.com/questions/96747/area-code-not-set-issue-in-custom-cli-commands-in-magento-2)
[http://magento-quickies.alanstorm.com/post/142652104930/magento-2-fixing-area-code-not-set-exceptions](http://magento-quickies.alanstorm.com/post/142652104930/magento-2-fixing-area-code-not-set-exceptions)

```shell
[Magento\Framework\Exception\SessionException]
Area code not set: Area code must be set before starting a session.
```
在构造函数中添加 Area code 定义

```php
    public function __construct(
        \Magento\Framework\App\State $state,
        $name=null
    )
    {
        $state->setAreaCode('frontend'); // or 'adminhtml', depending on your needs
        parent::__construct($name);
    }
```

