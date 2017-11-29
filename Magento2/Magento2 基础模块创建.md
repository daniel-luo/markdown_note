##Magento2 基础模块创建##

**文件结构**

```text
Tenf
 |
 |--First
 |   |
 |   |--registration.php
 |   |
 |   |--etc
 |   |   |--module.xml
 |   |   |--frontend
 |   |   |   |--routes.xml
 |   |
 |   |--Controller
 |   |   |--Index
 |   |   |   |--Index.php
 |   |       |--NewFolder
 |   |           |--Test.php
 |   |
 |   |--view
 |   |   |--frontend
 |   |   |   |--test.phtml
 |

```

a. 添加 `app/code/Tenf/First/etc/module.xml`
**注意:**
**noNamespaceSchemaLocation** 可以理解为命名空间根目录，如果命名空间没有生效，则会在这个定义的命名空间去找` Magento2 定义不可改`
**name** 是当前模块的命名空间，也可以理解为文件夹结构，且大小写应该和文件夹名字一致 `自定义,唯一`
**setup_version** 当前模块的版本，`自定义`

```xml
<?xml version="1.0"?>

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Tenf_First" setup_version="1.0.0">
    </module>
</config>
```

b. 添加 `app/code/Tenf/First/registration.php`, 对模块进行注册
**Function:** `register(注册类型,组件名字, 组件绝对路径)`
**module**, 是模块的注册类型，更多的注册类型详见 `vendor/magento/framework/Component/ComponentRegistrar.php`, 不同的注册类型 `moudle.xml` 中的 `noNamespaceSchemaLocation` 应该是不一样的
**Tenf_First** 是要注册的模块名字, 注意大小写和 `module.xml` 中需要保持一致
**__DIR__** 就是当前模块所在位置

```php
<?php

\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'Tenf_First',
    __DIR__
);
```

**模块注册信息添加完后我们需要在命令行中运行一下语句进行注册**

```shell
php bin/magento setup:upgrade
```

c. 为自定义的模块添加前端路由 `app/code/Tenf/First/etc/frontend/routes.xml`
**noNamespaceSchemaLocation** 注意这里和 `module.xml` 里的值是不一样的，且都有 `Magento2` 预定义，不能修改
**id** 最外层的 **router** `id` 前端 (frontend) 路径值一般为 `standard`
**id** 中间层的 **route** `id` 为自定义模块标识，需要是唯一的
**frontName** 前端访问模块时路径的前缀，自定义
**name** `module name` 和 `module.xml` 里定义的 `name` 是相对应的
**注意:** `Magento2 URL` 的规则是　`frontName/folderName/controllerFile`, 如果有子目录，子目录用 `_` 链接, 例如 `first/Index_NewFolder/index`, 在 `URL` 中不去分大小写

```xml
<?xml version="1.0"?>

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="standard">
        <route id="first" frontName="first">
            <module name="Tenf_First" />
        </route>
    </router>
</config>
```

d. 创建对应的 **Controller** 文件 `app/code/Tenf/First/Controller/Index/Index.php`

```php
<?php

namespace Tenf\First\Controller\Index;

use Magento\Framework\App\Action\Context;

class Index extends \Magento\Framework\App\Action\Action
{
    protected $_resultPageFactory;

    public function __construct(Context $context, \Magento\Framework\View\Result\PageFactory $resultPageFactory)
    {
        $this->_resultPageFactory = $resultPageFactory;
        parent::__construct($context);
    }

    public function execute()
    {
        $resultPage = $this->_resultPageFactory->create();
        return $resultPage;
    }
}
```

e. 添加 `layout` 布局文件 `app/code/Tenf/First/view/frontend/layout/first_index_index.xml`
**noNamespaceSchemaLocation** `Magento2` 预定义，不可修改
**layout** 预定义可选，可选值有 `1column`, `2columns`, `3columns` ...
**block** 需要渲染的 block
**布局文件命名规则** `frontName_folderName_ControllerFile`, 如果有子目录，子目录用 `_` 链接 `first_index_newfolder_test`

```xml
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="../../../../../../../lib/internal/Magento/Framework/View/Layout/etc/page_configuration.xsd" layout="1column">
    <body>
        <referenceContainer name="content">
            <block class="Tenf\First\Block\Test" name="test_block" template="test.phtml" />
        </referenceContainer>
    </body>
</page>
```

f. 添加自定义 Block, `app/code/Tenf/First/Block/Test.php`

```php
<?php
namespace Tenf\First\Block;

class Test extends \Magento\Framework\View\Element\Template
{
    public function test()
    {
        return 'Hello world!';
    }
}
```

g. 添加模板文件 `app/code/Tenf/First/view/frontend/templates/test.phtml`

```html
<h1><?php echo $this->test(); ?></h1>
```

**注意:** 测试的时候记得先清空缓存

```php
php bin/magento cache:clean
```

##Documents##
[http://inchoo.net/magento-2/how-to-create-a-basic-module-in-magento-2/](http://inchoo.net/magento-2/how-to-create-a-basic-module-in-magento-2/)






