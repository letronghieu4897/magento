![alt text](https://image.flaticon.com/icons/svg/818/818080.svg) 
# Linux Server

# System 
* Set mode : 
```sh

$ php bin/magento deploy:mode:show

$ php bin/magento deploy:mode:set developer

$ php bin/magento deploy:mode:set production

``` 

* Clear cache :
```sh

$ php bin/magento cache:status

$ php bin/magento cache:clean

$ php bin/magento cache:flush

$ php bin/magento cache:disable CACHE_TYPE

$ php bin/magento cache:enable CACHE_TYPE

``` 

* Indexer : 
```sh

$ php bin/magento indexer:info

$ php bin/magento indexer:status [indexer]

$ php bin/magento indexer:reindex [indexer]

``` 
# Front-end 
## 
# Back-end
## Install Database or Schema
* Create folder Setup : [Vendor]/[Extention]/Setup 
* Insert Data into Database 
  * Create file : **InstallData.php**
  * Create Schema or Insert New Column : **InstallSchema.php**
```php
<?php
namespace Netpower\Ghn\Setup;
use \Magento\Framework\Setup\InstallSchemaInterface;
use \Magento\Framework\Setup\SchemaSetupInterface;
use \Magento\Framework\Setup\ModuleContextInterface;
use \Magento\Framework\DB\Ddl\Table;

class InstallSchema implements InstallSchemaInterface
{

	public function install(SchemaSetupInterface $setup,ModuleContextInterface $context)
	{
		$installer = $setup;
		$installer->startSetup();

		if (!$installer->tableExists('[table_name]')) {
			$table = $installer->getConnection()->newTable(
				$installer->getTable('[table_name]')
			)
				->addColumn(
					'district_id',
					Table::TYPE_INTEGER,
					null,
					[
						'identity' => true,
						'nullable' => false,
						'primary'  => true,
						'unsigned' => true,
					],
					'District ID'
				)->addColumn(
					'name',
					Table::TYPE_TEXT,
					null,
					['nullable' => false],
					'District Name'
				)->addColumn(
					'region_id',
					Table::TYPE_INTEGER,
					null,
					['nullable' => false,
						'unsigned' => true,
					],
					'Region ID'
				)->addColumn(
					'province_id',
					Table::TYPE_INTEGER,
					null,
					['nullable' => false],
					'Province ID'
				)->addColumn(
					'district_number',
					Table::TYPE_INTEGER,
					null,
					['nullable' => false],
					'District Number'
				)->addForeignKey(
      		$installer->getFkName('[table_name]', 'region_id', '[table_name (foreign key) ]', 'region_id'),
      'region_id',
      $installer->getTable('directory_country_region'),
      'region_id',
      Table::ACTION_CASCADE
			);
			$installer->getConnection()->createTable($table);
		}
	}
}
```
* 
