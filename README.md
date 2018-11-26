![alt text](https://image.flaticon.com/icons/svg/818/818080.svg) 
# Linux Server
* Change user : **chown -R [username].[groupname] /[path]** : chown -R hieunetpower.hieunetpower /var/www/
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
# Admin site 
* Create new Menu : **app/code/[Vendor]/[Extention]/etc/adminhtml/menu.xml**
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Backend:etc/menu.xsd">
    <menu>
        <add id="Netpower_Ghn::ghn_order"
             title="GHN Orders"
             module="Magento_Backend"
             sortOrder="5"
             action="menuitem/index/index"
	     //parent="Magento_Sales::sales"
             resource="Magento_Backend::content"
        />
    </menu>
</config>
```
* Finding Parent : **../vendor/magento/module-sales/etc/adminhtml/menu.xml**

* Support for finding parent
```javascript
[1] System  (Magento_Backend::system)
[2] Dashboard   (Magento_Backend::dashboard)
[3] System  (Magento_Backend::system)
[4] Marketing   (Magento_Backend::marketing)
[5] Content (Magento_Backend::content)
[6] Stores  (Magento_Backend::stores)
[7] Products    (Magento_Catalog::catalog)
[8]     (Magento_Backend::system_currency)
[9] Customers   (Magento_Customer::customer)
[10] Find Partners & Extensions (Magento_Marketplace::partners)
[11] Reports    (Magento_Reports::report)
[12] Sales  (Magento_Sales::sales)
[13] UMC    (Umc_Base::umc)
 ()] 1
Use [Magento_Backend::system] as parent? (Y/N) (N)] N
Select Parent Menu: 
[1] Report  (Magento_Backend::system_report)
[2] Tools   (Magento_Backend::system_tools)
[3] Data Transfer   (Magento_Backend::system_convert)
[4] Other Settings  (Magento_Backend::system_other_settings)
[5] Extensions  (Magento_Integration::system_extensions)
[6] Permissions (Magento_User::system_acl)
```
# Front-end 
## 
# Back-end
## Install Database or Schema
* Create folder Setup : app/code/[Vendor]/[Extention]/Setup 
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
