##Magento2 升级脚本添加字段##
**File:**`Setup/UpgradeSchema.php`

```php
<?php

namespace Silksoftware\Suitcartwizard\Setup;

use Magento\Framework\Setup\UpgradeSchemaInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\SchemaSetupInterface;


class UpgradeSchema implements UpgradeSchemaInterface
{
  
public function upgrade(
    SchemaSetupInterface $setup,
    ModuleContextInterface $context
    ) {
     $installer = $setup;
   $installer->startSetup();

    if (version_compare($context->getVersion(), '1.0.1') < 0)
     {
            $table  = $installer->getConnection()
            ->newTable($installer->getTable('accessories_items'))
            ->addColumn(
                'entity_id',
                \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                null,
                ['identity' => true, 'unsigned' => true, 'nullable' => false, 'primary' => true],
                'Id'
            )
            ->addColumn(
                'parent_item_id',
                \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                null,
                ['nullable' => false],
                'Parent Item Id'
            )->addColumn(
                'parent_product_id',
                \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                null,
                ['nullable' => false],
                'Parent Product Id'
            )->addColumn(
                'category_id',
                \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                null,
                ['nullable' => false],
                'Category Id'
            )->addColumn(
                'child_item_id',
                \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                null,
                ['nullable' => false],
                'Accesory Item Id'
            )->addColumn(
                'child_product_id',
                \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                null,
                ['nullable' => false],
                'Accesory Product Id'
            )->addColumn(
                'quote_id',
                \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                null,
                ['nullable' => false],
                'Quote Id'
            )->addColumn(
                'price',
                \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                null,
                ['default' => 0.00],
                'Price'
            );
        $installer->getConnection()->createTable($table);
    }

    $this->_102Version($installer, $context);

    $installer->endSetup();
}

    protected function _102Version($installer, $context)
    {
        if (version_compare($context->getVersion(), '1.0.2', '<')) {
            $connection = $installer->getConnection();

            $connection->addColumn($installer->getTable('accessories_items'), 'position', array(
                        'type' => \Magento\Framework\DB\Ddl\Table::TYPE_INTEGER,
                        'length' => 11,
                        'comment' => 'Position'
                    ));

            $connection->addColumn($installer->getTable('accessories_items'), 'image', array(
                        'type' => \Magento\Framework\DB\Ddl\Table::TYPE_TEXT,
                        'length' => 255,
                        'comment' => 'Image'
                    ));
        }
    }
}
````