##Magento编辑订单实时核算价格位置##
1. `public_html/app/code/local/Mage/Sales/Model/Quote.php L1360` **collectTotals**, `$address->collectTotals();`
2. `$address` 是 `Mage_Sales_Model_Quote_Address` Line 1109 `collectTotals`

在 Mage_Sales_Model_Quote_Address 的 collectTotals 里边会计算所有的价格

##Mage_SalesRule_Model_Quote_Discount##
**每个产品的打折价格计算**
1. `parent::collect($address);` 计算 items 列表下小计
2. 自身方法　`collect`　计算该产品打折价格
3. 最后会在　`_aggregateItemDiscount` 这个方法中计算折扣价格

**Event**
`sales_quote_address_discount_item` 通过这个事件可以设置订单中每个 item 的优惠价格

**总结:**
`Mage_SalesRule_Model_Quote_Discount`的｀collect｀方法里可以设置最终的`运费`和`每个产品`减少的价格



####折扣####
`sales_flat_quote_address`

| 字段 | 类型 | 备注 |
|--------|-------|--------|
|discount_amount|负数|当前货币折扣了多少钱|
|base_discount_amount|负数|基本货币折扣了多少钱|

`sales_flat_quote_item`

| 字段 | 备注 |
|--------|--------|
|discount_amount|一共折扣了多少钱，正数|
|base_discount_amount|基本货币折扣了多少钱|