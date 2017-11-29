##Magento 后台表格时间抓取##

```php
        $fieldset->addField('time', 'date', array(
            'label'    => $this->__('Date In Produced'),
            'required' => true,
            'name'     => 'time',
            'image'    => $this->getSkinUrl('images/grid-cal.gif'),
            'format'   => Mage::app()->getLocale()->getDateFormat(Mage_Core_Model_Locale::FORMAT_TYPE_SHORT),
            'value'      => date( Mage::app()->getLocale()->getDateStrFormat(Mage_Core_Model_Locale::FORMAT_TYPE_SHORT),
                              strtotime('next weekday') )
        ));
```