##Magento2创建Controller
```text
Tenf
 |--Test
 |  |--Controller
 |  |  |--Index
 |  |  |  |--Index.php
 |  |  |
 |  |  |--Block
 |  |  |  |--Header.php
 |  |
 |  |--Block
 |  |  |--Content.php
 |  |
 |  |--View
 |  |  |--frontend
 |  |  |  |--layout
 |  |  |  |  |--test_index_index.xml
 |  |  |  |  |--test_block_index.xml
 |  |  |  |
 |  |  |  |--templates
 |  |  |  |  |--content.phtml
 |  |
 |  |--etc
 |  |  |--frontend
 |  |  |  |--routes.xml
 |  |  |--module.xml
 |  |
 |  |--registration.php
```

**1. registration.php**
```php
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'Tenf_Test',
    __DIR__
);
```

**2. etc/module.xml**
```xml
<?xml version="1.0"?>
<!--
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Tenf_Test" setup_version="0.0.1">
    </module>
</config>
```

**3. etc/frontend/routes.xml**
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="standard">
        <route id="tenf_test" frontName="test">
            <module name="Tenf_Test" />
        </route>
    </router>
</config>
```
`router/route->id` 自定义的唯一值
`router/route->frontName` 前端访问时的前缀
`router/route/module->name` `module.xml` 里定义的模块名

**4. view/frontend/layout/test_index_index.xml**
```xml
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" layout="1column" xsi:noNamespaceSchemaLocation="../../../../../../../lib/internal/Magento/Framework/View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceContainer name="content">
            <block class="Tenf\Test\Block\Content" name="customer.content" template="content.phtml" />
        </referenceContainer>
    </body>
</page>
```
Layout 文件名字 `frontName_DirName_FileName.xml`
访问地址: `test_index_index.xml` => `/test/index/index`; `test_block_index.xml` => `/test/block/index`

**5. Controller/Index/Index.php**
```php
namespace Tenf\Test\Controller\Index;


class Index extends \Magento\Framework\App\Action\Action
{
    /**
     * @var \Magento\Framework\View\Result\PageFactory
     */
    protected $resultPageFactory;

    /**
     * @param \Magento\Framework\App\Action\Context $context
     * @param \Magento\Framework\View\Result\PageFactory resultPageFactory
     */
    public function __construct(
        \Magento\Framework\App\Action\Context $context,
        \Magento\Framework\View\Result\PageFactory $resultPageFactory
    )
    {
        $this->resultPageFactory = $resultPageFactory;
        parent::__construct($context);
    }

    public function execute()
    {
        $page = $this->resultPageFactory->create();
        $page->addPageLayoutHandles(['index' => 'index'], 'test');
        return $page;
    }
}
```