##Magento2 Category getCollection 后直接像 Product List 一样 getProductCollectio##

```php
    public function __construct(
        \Magento\Framework\App\Action\Context $context,
        \Magento\Catalog\Model\Layer\Resolver $layerResolver,
        \Magento\Framework\View\Result\PageFactory $resultPageFactory
    )
    {
        $this->_context = $context;
        $this->_resultPageFactory = $resultPageFactory;
        $this->_catalogLayer = $layerResolver->get();
    }
```

```php
$categoryCollection = $objectManager->create('Magento\Catalog\Model\ResourceModel\Category\Collection');
$categoryCollection->addIdFilter($cat_ids)
                                ->addAttributeToSelect('name');

foreach ($this->getCategoryCollection() as $category) {
    $this->getLayer()->setCurrentCategory($category);
    $_productCollection = $this->_catalogLayer()->getProductCollection();
    foreach ($_productCollection as $_product) {
        print_r($_product->getData());
        exit;
        // $productImage = $block->getImage($_product, 'category_page_grid');
    }
}
```

**See** `vendor/magento/module-catalog/Block/Product/ListProduct.php` **Line 124**
