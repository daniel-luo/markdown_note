##Magento2 获取Block内容和头部css和js
```php
$page = $this->resultPageFactory->create();
$cPage = $this->_objectManager->create('Silk\Pimcore\Model\Page');
$page->addPageLayoutHandles(['index' => 'index'], 'optical-center');
echo '<html>';
echo '<head>';
echo $page->getLayout()->renderElement('require.js');
echo $cPage->renderHeadContent();
echo '</heade>';
echo '<body>';
echo $page->getLayout()->renderElement('after.body.start');
echo $page->getLayout()->renderElement('header.container');
exit;
```

Page.php
```php
namespace Silk\Pimcore\Model;

use Magento\Framework\View\Result\Page as MagePage;

class Page extends MagePage
{
    public function renderHeadContent()
    {
        return $this->pageConfigRenderer->renderHeadContent();
    }
}
```

##相关类
`Magento\Framework\View\Page`
`Magento\Framework\View\Page\Config`