##Magento 自定义 Procuct List 模块##
**Mage_Catalog_Block_Product_List.php**
原生大致逻辑:
1. List Block 先通过 Mage_Catalog_Model_Layer 模块得到 Collection, 这个 Collection 是从分类获取到的
2. Mage_Catalog_Model_Layer 模块在获取了 分类下产品的 Collection 后对他进行预处理 prepareProductCollection，添加查询属性各种 price , 然后通过 getFilterableAttributes 处理左边有哪些过滤项目
3. 最后在 Mage_Catalog_Block_Layer_View 要渲染前，通过方法 _prepareLayout 里的 setAttributeModel 修改 Collection 设置过滤项, 属性，动态数据，价格对应的 $attribute 类不一样
eg: Mage_Catalog_Model_Resource_Layer_Filter_Attribute 类中 applyFilterToCollection 会对产品列表页面的 collection 进行加工



**关键代码:**
```php
    /**
     * Retrieve loaded category collection
     *
     * @return Mage_Eav_Model_Entity_Collection_Abstract
     */
    protected function _getProductCollection()
    {
        /**
        $layer = $this->getLayer();

        return $layer->getProductCollection();
        //*/


        $layer = $this->getLayer();

        $collection = $layer->getProductCollection();
        $currentCategory = Mage::registry('current_category');

        $collection
            ->addAttributeToSelect(Mage::getSingleton('catalog/config')->getProductAttributes())
            ->addMinimalPrice()
            ->addFinalPrice()
            ->addTaxPercents()
            ->addUrlRewrite($currentCategory->getId());

		// 开始添加过滤项 参考 Mage_Catalog_Model_Resource_Layer_Filter_Attribute->applyFilterToCollection()
        $connection = Mage::getSingleton('core/resource')->getConnection('core_read');
        $tableAlias = 'color_idx';
        $conditions = array(
            "{$tableAlias}.entity_id = e.entity_id",
            $connection->quoteInto("{$tableAlias}.attribute_id = ?", 92),
            $connection->quoteInto("{$tableAlias}.store_id = ?", $collection->getStoreId()),
            $connection->quoteInto("{$tableAlias}.value = ?", 24)
        );

        $collection->getSelect()->join(
            array($tableAlias => 'catalog_product_index_eav'),
            implode(' AND ', $conditions),
            array()
        );

		// 开始添加第二个过滤项
        $connection = Mage::getSingleton('core/resource')->getConnection('core_read');
        $tableAlias = 'size_idx';
        $conditions = array(
            "{$tableAlias}.entity_id = e.entity_id",
            $connection->quoteInto("{$tableAlias}.attribute_id = ?", 180),
            $connection->quoteInto("{$tableAlias}.store_id = ?", $collection->getStoreId()),
            $connection->quoteInto("{$tableAlias}.value = ?", 230)
        );

        $collection->getSelect()->join(
            array($tableAlias => 'catalog_product_index_eav'),
            implode(' AND ', $conditions),
            array()
        );

        Mage::getSingleton('catalog/product_status')->addVisibleFilterToCollection($collection);
        Mage::getSingleton('catalog/product_visibility')->addVisibleInCatalogFilterToCollection($collection);
        $collection->setPageSize(2)->setCurPage(1);
        $collection->printLogQuery(true);
        return $collection;
         //*/
    }
```

####获取初步的分类 Collection
1. `Mage_Catalog_Block_Product_List->_getProductCollection()`
2. `Mage_Catalog_Model_Layer->getProductCollection()`
3. `Mage_Catalog_Model_Category->getProductCollection()`
4. `Mage_Catalog_Model_Resource_Product_Collection->addCategoryFilter()` 设置一些分类的规则
5. `Mage_Catalog_Model_Resource_Layer_Filter_Attribute->applyFilterToCollection()` 添加过滤规则