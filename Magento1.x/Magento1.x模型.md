##Magento Create Custom Moddel##
####结构####
```text
Tenf
|--Test
|   |--etc
|   |   |-config.xml
|   |
|   |--Model
|   |   |--Resource
|   |   |   |--First
|   |   |   |  |--Collection.php            Mage_Core_Model_Resource_Db_Collection_Abstract
|   |   |   |
|   |   |   |--First.php                    Mage_Core_Model_Resource_Db_Abstract
|   |   |   |
|   |   |   |
|   |   |   |--Second
|   |   |   |   |--Child
|   |   |   |   |   |--Collection.php
|   |   |   |   |
|   |   |   |   |--Child.php
|   |   |
|   |   |
|   |   |--First.php                        Mage_Core_Model_Abstract
|   |   |
|   |   |--Second
|   |   |   |--Child.php
|   |
|   |--setup
|   |   |--tenf_test_setup
|   |   |   |--mysql4-install-0.1.0.php            Mage_Core_Model_Resource_Setup
|   |   |   |--mysql4-upgrade-0.1.0-0.1.1.php
|   |
|   |--data
|   |   |--tenf_test_setup
|   |   |   |--data-install-0.1.0.php
|   |   |   |--data-upgrade-0.1.0-0.1.1.php
|   |

Root
|--app
|   |--etc
|   |   |--modules
|   |   |   |--Tenf_Test.xml
|   |
```

**1. Root/app/etc/modules/Tenf_Test.xml**
```xml

```

**2. Tenf/Test/etc/config.xml**
```xml
<?xml version="1.0"?>
<config>
    <modules>
        <Tenf_Test> <!-- 定义当前模块命名空间，同时也是模块文件夹结构，随便自定义但要保持唯一性 -->
            <version>0.1.0</version>
        </Tenf_Test>
    </modules>
    <global>
    	<models>
        	<tenf_test> <!-- 保持和Model命名空间一致，但是是小写 -->
            	<class>Tenf_Test_Model</class><!-- 存放Model类的文件夹 -->
                <resourceModel>tenf_test_resource</resourceModel><!-- 定义模块的资源文件节点，定义格式: 命名空间_resource -->
            </tenf_test>
            <tenf_test_resource><!-- 资源模型实现 -->
            	<class>Tenf_Test_Model_Resource</class><!-- 存放资源模型类的文件夹 -->
                <entities><!-- 定义模型对应的表, 节点名是 entity 的复数形式，表明不止一个实例 -->
                	<first><!-- 自定义表别名，用于Magneto寻找表，有唯一性 -->
                    	<table>first_table</table><!-- first_table 是要创建的表的名字 -->
                    </first>
                    <second>
                    	<table>second_table</table>
                    </second>
                </entities>
            </tenf_test_resource>
        </models>
        <resources>
        	<tenf_test_setup> <!-- 命名空间_setup -->
            	<setup>
                	<module>Tenf_Test</module> <!-- 哪个模块的setup文件 -->
                    <class>Mage_Core_Model_Resource_Setup</class> <!-- setup 类是用哪个 -->
                </setup>
            </tenf_test_setup>
        </resources>
    </global>
</config>
```

**3. Tenf/Test/Model/First.php**
```php
<?php
/**
 * @authors daniel
 * @date    17-3-14 上午9:46
 * @version 0.1.0
 */
class Tenf_Test_Model_First extends Mage_Core_Model_Abstract
{
    protected $_eventPrefix = 'tenf_test';

    protected function _construct()
    {
        // resource model
        $this->_init('tenf_test/first');
    }
}
```

**4. Tenf/Test/Model/Resource/First.php**
```php
<?php
/**
 * @authors daniel
 * @date    17-3-14 上午9:46
 * @version 0.1.0
 */
class Tenf_Test_Model_Resource_First extends Mage_Core_Model_Resource_Db_Abstract
{
    protected function _construct()
    {
        // table_name, pk
        $this->_init('tenf_test/first', 'id');
    }
}
```

**5. Tenf/Test/Model/Resource/First/Collection.php**
```php
<?php
/**
 * @authors daniel
 * @date    17-3-14 上午9:46
 * @version 0.1.0
 */
class Tenf_Test_Model_Resource_First_Collection extends Mage_Core_Model_Resource_Db_Collection_Abstract
{
    protected $_eventPrefix = 'tenf_test';

    protected function _construct()
    {
        // resource model
        $this->_init('tenf_test/first');
    }
}
```

**6. Tenf/Test/sql/tenf_test_setup/mysql4-install-0.1.0.php
```php
<?php
/**
 * @authors daniel
 * @date    17-3-14 上午9:46
 * @version 0.1.0
 */
$install = $this;
$install->startSetup();

$sql =<<<eod
CREATE TABLE `{$this->getTable('tenf_test/first')}` (
`id`  int UNSIGNED NOT NULL AUTO_INCREMENT ,
`name`  varchar(255) CHARACTER SET utf8 NULL ,
`age`  int UNSIGNED NULL ,
PRIMARY KEY (`id`)
)
;
eod;
$install->run($sql);

$install->endSetup();
```

**7. Tenf/Test/sql/tenf_test_setup/mysql4-upgrade-0.1.0-0.1.1.php**
```php
<?php
/**
 * @authors daniel
 * @date    17-3-14 上午9:46
 * @version 0.1.0
 */
$install = $this;
$install->startSetup();

$sql =<<<eod
ALTER TABLE `{$this->getTable('tenf_test/first')}`
ADD COLUMN `active`  int UNSIGNED NULL AFTER `age`;
eod;
$install->run($sql);

$install->endSetup();
```

**8. Tenf/Test/data/tenf_test_setup/data-install-0.1.0.php**
```php
<?php
/**
 * @authors daniel
 * @date    17-3-14 上午9:46
 * @version 0.1.0
 */
$customers = array(
    array(
        'name' => 'Admin',
        'age' => 30,
    ),
    array(
        'name' => 'Dev',
        'age' => 25,
    ),
);

foreach ($customers as $customer) {
    Mage::getModel('tenf_test/first')
        ->setData($customer)
        ->save();
}
```

**8. Tenf/Test/data/tenf_test_setup/data-upgrade-0.1.0-0.1.1.php**
```php
<?php
/**
 * @authors daniel
 * @date    17-3-14 上午9:46
 * @version 0.1.0
 */
$customers = Mage::getModel('tenf_test/first')
    ->getCollection();

foreach ($customers as $customer) {
    $customers->setActive(1)
        ->save();
}
```



##Magento1.x寻找表名逻辑##
`app/code/core/Mage/Core/Model/Resource.php`
**getTableName**

