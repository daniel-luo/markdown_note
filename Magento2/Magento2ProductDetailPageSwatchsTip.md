##Magento2 Product Detail Page Swatchs Tip##
**测试环境: Magento2 EE 2.10**
##问题描述##
在 Magento2 中创建一个 Configurable 产品，包含属性 `color` 和 `size`, 但产品详情页面 `size` 的 Option 不显示只在页面显示一个 size。
到后台修改 `store > Attributes > Product`　查询属性 `size`. `Properties > Catalog Input Type for Store Owner` 改为　`Dropdown` 产品详情页面可以显示 `Dropdown` 的 `size` 选项。

##解决办法:##
如果 `Dropdown`　和 `Visual Swatch` 要显示, `TEXT Swatch` 不显示，造成的原因是 Swatch 的值是空的，需要到后台去为当前属性添加上　swatch　的文本信息即可。
`Dropdown` 的值来自于 `eav_attribute_option_value`, 其他两个的值来自于 `eav_attribute_option_swatch`, 其中如果 `Swatch` 的值没有被设定，前端产品详情页面是不会显示该属性的选项的!

*****

##工作原理##
在 Magento2 中相对 Magento1 多了一张表 `eav_attribute_option_swatch` 该表主要用于记录属性值在产品详情页面的显示方式，当该属性的 `Catalog Input Type for Store Owner` 为非 `Dropdown` 的时候 Magento2 会访问这张表查找该属性的显示方式:
**代码位置:**`vendor/magento/module-swatches/Helper/Data.php` function **getSwatchesByOptionsId**

```php
    /**
     * Get swatch options by option id's according to fallback logic
     *
     * @param array $optionIds
     * @return array
     */
    public function getSwatchesByOptionsId(array $optionIds)
    {
        /** @var \Magento\Swatches\Model\ResourceModel\Swatch\Collection $swatchCollection */
        $swatchCollection = $this->swatchCollectionFactory->create();
        $swatchCollection->addFilterByOptionsIds($optionIds);

        $swatches = [];
        $currentStoreId = $this->storeManager->getStore()->getId();
        foreach ($swatchCollection as $item) {
        	//
            // 数据见面数据表
        	// 注意这里 $swatchCollection 只会取到前三条数据 37, 38, 39
            // 所以这里如果你先设置了 Catalog Input Type for Store Owner 为 Visual Swatch
            // 然后在将设置改为 TEXT Swatch 他只会选到 37,38,39 的值
            //
            if ($item['type'] != Swatch::SWATCH_TYPE_TEXTUAL) {
                $swatches[$item['option_id']] = $item->getData();
            } elseif ($item['store_id'] == $currentStoreId && $item['value']) {
                $fallbackValues[$item['option_id']][$currentStoreId] = $item->getData();
            } elseif ($item['store_id'] == self::DEFAULT_STORE_ID) {
                $fallbackValues[$item['option_id']][self::DEFAULT_STORE_ID] = $item->getData();
            }
        }

        if (!empty($fallbackValues)) {
            $swatches = $this->addFallbackOptions($fallbackValues, $swatches);
        }
        return $swatches;
    }
```
Tip:
```php
DEFAULT_STORE_ID == 0
SWATCH_TYPE_TEXTUAL == 0
```

属性表中事例数据
```sql
mysql>  SELECT `main_table`.* FROM `eav_attribute_option_swatch` AS `main_table` WHERE (`main_table`.`option_id` IN(16, 17, 18));
```
**数据表**

| swatch_id | option_id | store_id | type | value |
|-----------|-----------|----------|------|-------|
|        37 |        16 |        0 |    3 | NULL  |
|        38 |        17 |        0 |    3 | NULL  |
|        39 |        18 |        0 |    3 | NULL  |
|        40 |        16 |        1 |    0 | NULL  |
|        41 |        17 |        1 |    0 | NULL  |
|        42 |        18 |        1 |    0 | NULL  |



`vendor/magento/module-swatches/view/frontend/templates/product/view/renderer.phtml`
