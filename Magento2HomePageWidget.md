##Magento2 HomePage Widget##

在页面上加入一个 Block 例如： Best Saler 可以通过在 Homepage Content 中加入一个 Widget, 这个 Widget 的属性 **conditions_encoded** 可以指定过滤条件;
改属性的值是一个处理过的序列化的数组，代码详见: `vendor/magento/module-widget/Helper/Conditions.php`

**Block**
```text
{{widget type="Magento\\CatalogWidget\\Block\\Product\\ProductsList" products_per_page="20" products_count="20" template="Magento_Catalog::product/bestseller_list.phtml" conditions_encoded="a:2:[i:1;a:4:[s:4:`type`;s:50:`Magento|CatalogWidget|Model|Rule|Condition|Combine`;s:10:`aggregator`;s:3:`all`;s:5:`value`;s:1:`1`;s:9:`new_child`;s:0:``;]s:4:`1--1`;a:4:[s:4:`type`;s:50:`Magento|CatalogWidget|Model|Rule|Condition|Product`;s:9:`attribute`;s:3:`sku`;s:8:`operator`;s:2:`()`;s:5:`value`;s:258:`GC_800-0S2,JM_15-1790,GC_443FRD-TCL00,GC_S880-XS2POM,GC_320-T0S2FS,HF_24931,GC_900-0S2,GC_445RNY-TCR02,GC_S844-XS2POFN,GC_320-T0S2FS,JM_20-8471,GC_S871-XS2POM,CKS_40ZK808,TCC_5995,GC_900-OS2,GC_S842-XS2M,SP_TEDDYBOY2,LVS_49931-0115,SP_SUBSPACE,LVS_49931-0133`;]]"}}
```

**conditions_encoded**
**1** 必须要! 否则 **1--1** 不会生效，即使将 `1--1` 改为　`1` 属性 `sku` 的过滤条件也不会生效!

```php
$condition = array (
  1 => 
  array (
    'type' => 'Magento\\CatalogWidget\\Model\\Rule\\Condition\\Combine',
    'aggregator' => 'all',
    'value' => '1',
    'new_child' => '',
  ),
  '1--1' => 
  array (
    'type' => 'Magento\\CatalogWidget\\Model\\Rule\\Condition\\Product',
    'attribute' => 'sku',
    'operator' => '()',
    'value' => 'GC_800-0S2,JM_15-1790,GC_443FRD-TCL00,GC_S880-XS2POM,GC_320-T0S2FS,HF_24931,GC_900-0S2,GC_445RNY-TCR02,GC_S844-XS2POFN,GC_320-T0S2FS,JM_20-8471,GC_S871-XS2POM,CKS_40ZK808,TCC_5995,GC_900-OS2,GC_S842-XS2M,SP_TEDDYBOY2,LVS_49931-0115,SP_SUBSPACE,LVS_49931-0133',
  ),
);
```

**vendor/magento/module-widget/Helper/Conditions.php**

```php
<?php
/**
 * Copyright © 2016 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
namespace Magento\Widget\Helper;

/**
 * Widget Conditions helper
 */
class Conditions
{
    /**
     * Encode widget conditions to be used with WYSIWIG
     *
     * @param array $value
     * @return string
     */
    public function encode(array $value)
    {
        $value = str_replace(['{', '}', '"', '\\'], ['[', ']', '`', '|'], serialize($value));
        return $value;
    }

    /**
     * Decode previously encoded widget conditions
     *
     * @param string $value
     * @return array
     */
    public function decode($value)
    {
        $value = str_replace(['[', ']', '`', '|'], ['{', '}', '"', '\\'], $value);
        $value = unserialize($value);
        return $value;
    }
}

```