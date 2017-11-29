注意：使用 `funciton sendTransactional()` 发送邮件必须设置 `sender name`, `sender email`, `template subject` 否则无法通过邮件验证 **app/code/core/Mage/Core/Model/Email/Template.php Line 261 function isValidForSend()**, 最终无法发送邮件。 
```php
    public function addEmailQueue($orderId, $templateCode, $senderInfo, $toInfo, $params, $subject, $entityType = 'order', $eventType = 'new_order')
    {
        $order = Mage::getModel('sales/order')->load($orderId);
        $storeId = $order->getStore()->getId();

        // queue entity
        /** @var $emailQueue Mage_Core_Model_Email_Queue */
        $emailQueue = Mage::getModel('core/email_queue');
        $emailQueue->setEntityId($orderId)
            ->setEntityType($entityType)
            ->setEventType($eventType);


        $emailTemplate = Mage::getModel('core/email_template');
        $emailTemplate->setQueue($emailQueue);
        $emailTemplate->setTemplateSubject($subject);
        $emailTemplate->sendTransactional($templateCode, $senderInfo, $toInfo['email'], $toInfo['name'], $params, $storeId);
    }
```

使用 eg:

```php
Mage::helper('silk_email')->addEmailQueue(
    $order->getId(),
    'silk_shipping_tracking',
    array(
            'email' => Mage::getStoreConfig('trans_email/ident_general/email', $storeId),
            'name' => Mage::getStoreConfig('trans_email/ident_general/name', $storeId)
    ),
    array(
        'email' => $order->getCustomerEmail(),
        'name' => $order->getCustomerName()
    ),
    $params,
    ' ',
    'order',
    'shipping'
);
```

邮件模板添加：**config.xml**，原理见 `Mage_Core_Model_Email_Template getDefaultTemplates()`
```xml
  <global>
        <template>
            <email>
                <sales_email_retailer_template translate="label" module="silk_retailer">
                    <label>Retailer Confirmation Template</label>
                    <file>silk/retailer_confirmation.html</file>
                    <type>html</type>
                </sales_email_retailer_template>
            </email>
        </template>
    </global>
```
**注意:**
模板中 `email` 下的节点 `sales_email_retailer_template`　需要和 `system.xml` 中对应，才能使用后台的自定义邮件模板功能 `system --> Transactional Emails`

**system.xml**
```xml
<?xml version="1.0"?>
<config>
    <sections>
        <sales_email translate="label" module="sales">
            <groups>
                <retailer translate="label">
                    <label>Retailer Confirmation Template</label>
                    <frontend_type>text</frontend_type>
                    <sort_order>1</sort_order>
                    <show_in_default>1</show_in_default>
                    <show_in_website>1</show_in_website>
                    <show_in_store>1</show_in_store>
                    <fields>
                        <enabled translate="label">
                            <label>Enabled</label>
                            <frontend_type>select</frontend_type>
                            <source_model>adminhtml/system_config_source_yesno</source_model>
                            <sort_order>0</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </enabled>
                        <identity translate="label">
                            <label>Retailer Confirmation Email Sender</label>
                            <frontend_type>select</frontend_type>
                            <source_model>adminhtml/system_config_source_email_identity</source_model>
                            <sort_order>1</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </identity>
                        <template translate="label">
                            <label>Retailer Confirmation Template</label>
                            <frontend_type>select</frontend_type>
                            <source_model>adminhtml/system_config_source_email_template</source_model>
                            <sort_order>2</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </template>
                    </fields>
                </retailer>
            </groups>
        </sales_email>
    </sections>
</config>
```