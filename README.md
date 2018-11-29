![Mọi](https://image.flaticon.com/icons/svg/818/818080.svg) 
![Beginner](https://img.shields.io/badge/Magento-Beginner-brightgreen.svg)
![Server](https://img.shields.io/badge/Magento-CetOS-red.svg)
![System](https://img.shields.io/badge/Magento-System-brightgreen.svg)
![Admin Site](https://img.shields.io/badge/Magento-Admin-red.svg)
![Front-end](https://img.shields.io/badge/Magento-Frontend-brightgreen.svg)
![Back-end](https://img.shields.io/badge/Magento-Backend-red.svg)

# TABLE CONTENTs
**I. Linux Server** 
 - **1**. Change user
 
**II. System**
- **1**. Set mode
- **2**. Clear cache
- **3**. Indexer

**III. Admin Site**

- **1**. Create new menu
- **2**. Create Grid on admin
- **3**. Create Collection

**IV. Front-end**
- **1**. 

**V. Back-end**
- **1**. Install Database or Schema
- **2**. Cron Job 

# I. Linux Server
## **1**. Change user 
* **chown -R [username].[groupname] /[path]** : chown -R hieunetpower.hieunetpower /var/www/
# II.System 
## **1**. Set mode : 
```sh

$ php bin/magento deploy:mode:show

$ php bin/magento deploy:mode:set developer

$ php bin/magento deploy:mode:set production

``` 

## **2**. Clear cache :
```sh

$ php bin/magento cache:status

$ php bin/magento cache:clean

$ php bin/magento cache:flush

$ php bin/magento cache:disable CACHE_TYPE

$ php bin/magento cache:enable CACHE_TYPE

``` 

## **3**. Indexer : 
```sh

$ php bin/magento indexer:info

$ php bin/magento indexer:status [indexer]

$ php bin/magento indexer:reindex [indexer]

``` 
# III.Admin site 
## **1**. Create new Menu 
* **app/code/[Vendor]/[Extention]/etc/adminhtml/menu.xml**
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
### **1.1** Finding Parent 
* **../vendor/magento/module-sales/etc/adminhtml/menu.xml**

### **1.2** Support for finding parent
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

## **2**. Create Grid on Admin 
```bash
$ TREE
. _______________________________________________________________________
├── etc
│     └── di.xml	
├── view
│     └── adminhtml
|	   └── layout 
|		└── ghn_ghn_index.xml
├── Controller
|     └── Adminhtml
|	   └── Ghn
|		└── Index.php
├── Block
│     └── Adminhtml
│   	   └── Order.php
│   
├── Model
|     ├── GhnOrder.php
|     └── ResourceModel
|		├── GhnOrder.php
|		└── GhnOrder 
|			└── Collection.php
|
. ________________________________________________________________________
```
### **2.1** Declared on di.xml 
* **\app\code\[Vendor]\[Extention]\etc\di.xml**
```xml
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Magento\Framework\View\Element\UiComponent\DataProvider\CollectionFactory">
        <arguments>
            <argument name="collections" xsi:type="array">
                <item name="netpower_ghn_post_listing_data_source" xsi:type="string">Netpower\Ghn\Model\ResourceModel\GhnOrder\Collection</item>
            </argument>
        </arguments>
    </type>
    <virtualType name="Netpower\Ghn\Model\ResourceModel\Post\Grid\Collection" type="Magento\Framework\View\Element\UiComponent\DataProvider\SearchResult">
        <arguments>
            <argument name="mainTable" xsi:type="string">netpower_ghn_ghn</argument>
            <argument name="resourceModel" xsi:type="string">Netpower\Ghn\Model\ResourceModel\GhnOrder</argument>
        </arguments>
    </virtualType>
</config>
```
### **2.2** Create layout Grid 
* **\app\code\[Vendor]\[Extention]\view\adminhtml\layout\ghn_ghn_index.xml**
```xml 
<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <update handle="styles"/>
    <body>
        <referenceContainer name="content">
            <block class="Netpower\Ghn\Block\Adminhtml\Order" name="ghn_grid">
                <block class="Magento\Backend\Block\Widget\Grid" name="ghn_grid.grid" as="grid">
                    <arguments>
                        <argument name="id" xsi:type="string">ghn_order_id</argument>
                         <argument name="dataSource" xsi:type="object">Netpower\Ghn\Model\ResourceModel\GhnOrder\Collection</argument>
                        <argument name="default_sort" xsi:type="string">id</argument>
                        <argument name="default_dir" xsi:type="string">ASC</argument>
                        <argument name="save_parameters_in_session" xsi:type="string">1</argument>
                    </arguments>
                    <block class="Magento\Backend\Block\Widget\Grid\ColumnSet" name="ghn_grid.grid.columnSet" as="grid.columnSet">
                        <arguments>
                            <argument name="rowUrl" xsi:type="array">
                                <item name="path" xsi:type="string">*/*/edit</item>
                            </argument>
                        </arguments>
                        <block class="Magento\Backend\Block\Widget\Grid\Column" as="ghn_order_id">
                            <arguments>
                                <argument name="header" xsi:type="string" translate="true">ID GHN</argument>
                                <argument name="index" xsi:type="string">ghn_order_id</argument>
                                <argument name="type" xsi:type="string">text</argument>
                                <argument name="column_css_class" xsi:type="string">col-id</argument>
                                <argument name="header_css_class" xsi:type="string">col-id</argument>
                            </arguments>
                        </block>
                        <block class="Magento\Backend\Block\Widget\Grid\Column" as="status">
                            <arguments>
                                <argument name="header" xsi:type="string" translate="true">Status</argument>
                                <argument name="index" xsi:type="string">status</argument>
                                <argument name="type" xsi:type="string">text</argument>
                                <argument name="column_css_class" xsi:type="string">col-id</argument>
                                <argument name="header_css_class" xsi:type="string">col-id</argument>
                            </arguments>
                        </block>
                        <block class="Magento\Backend\Block\Widget\Grid\Column" as="shipping_address">
                            <arguments>
                                <argument name="header" xsi:type="string" translate="true">Shipping Address</argument>
                                <argument name="index" xsi:type="string">shipping_address</argument>
                                <argument name="type" xsi:type="string">text</argument>
                                <argument name="column_css_class" xsi:type="string">col-id</argument>
                                <argument name="header_css_class" xsi:type="string">col-id</argument>
                            </arguments>
                        </block>
                        <block class="Magento\Backend\Block\Widget\Grid\Column" as="service_name">
                            <arguments>
                                <argument name="header" xsi:type="string" translate="true">Service Name</argument>
                                <argument name="index" xsi:type="string">service_name</argument>
                                <argument name="type" xsi:type="string">text</argument>
                                <argument name="column_css_class" xsi:type="string">col-id</argument>
                                <argument name="header_css_class" xsi:type="string">col-id</argument>
                            </arguments>
                        </block>
                        <block class="Magento\Backend\Block\Widget\Grid\Column" as="service_cost">
                            <arguments>
                                <argument name="header" xsi:type="string" translate="true">Service Cost</argument>
                                <argument name="index" xsi:type="string">service_cost</argument>
                                <argument name="type" xsi:type="string">text</argument>
                                <argument name="column_css_class" xsi:type="string">col-id</argument>
                                <argument name="header_css_class" xsi:type="string">col-id</argument>
                            </arguments>
                        </block>
                        <block class="Magento\Backend\Block\Widget\Grid\Column" as="order_code">
                            <arguments>
                                <argument name="header" xsi:type="string" translate="true">Order Code</argument>
                                <argument name="index" xsi:type="string">order_code</argument>
                                <argument name="type" xsi:type="string">text</argument>
                                <argument name="column_css_class" xsi:type="string">col-id</argument>
                                <argument name="header_css_class" xsi:type="string">col-id</argument>
                            </arguments>
                        </block>
                        <block class="Magento\Backend\Block\Widget\Grid\Column" as="shipping_order_id">
                            <arguments>
                                <argument name="header" xsi:type="string" translate="true">Shipping Order ID</argument>
                                <argument name="index" xsi:type="string">shipping_order_id</argument>
                                <argument name="type" xsi:type="string">text</argument>
                                <argument name="column_css_class" xsi:type="string">col-id</argument>
                                <argument name="header_css_class" xsi:type="string">col-id</argument>
                            </arguments>
                        </block>
                        <block class="Magento\Backend\Block\Widget\Grid\Column" as="order_id">
                            <arguments>
                                <argument name="header" xsi:type="string" translate="true">Order ID</argument>
                                <argument name="index" xsi:type="string">order_id</argument>
                                <argument name="type" xsi:type="string">text</argument>
                                <argument name="column_css_class" xsi:type="string">col-id</argument>
                                <argument name="header_css_class" xsi:type="string">col-id</argument>
                            </arguments>
                        </block>
                    </block>
                </block>
            </block>
        </referenceContainer>
    </body>
</page>
```
### **2.3** Create Controller to get action show Layout 
* **\app\code\[Vendor]\[Extention]\Controller\Adminhtml\Ghn\Index.php**
```php
<?php

namespace Netpower\Ghn\Controller\Adminhtml\Ghn;

class Index extends \Magento\Backend\App\Action
{
    protected $resultPageFactory = false;

    public function __construct(
        \Magento\Backend\App\Action\Context $context,
        \Magento\Framework\View\Result\PageFactory $resultPageFactory
    )
    {
        parent::__construct($context);
        $this->resultPageFactory = $resultPageFactory;
    }

    public function execute()
    {   

        $resultPage = $this->resultPageFactory->create();
        $resultPage->getConfig()->getTitle()->prepend((__('GHN Order')));

        return $resultPage;

    }

}
```
### **2.4** Create Block for admin Grid 
* **\app\code\[Vendor]\[Extention]\Block\Adminhtml\Order.php**
```php
<?php
namespace Netpower\Ghn\Block\Adminhtml;

class Order extends \Magento\Backend\Block\Widget\Grid\Container
{
	protected function _construct()
	{
		$this->_controller = 'adminhtml_order';
		$this->_blockGroup = 'Netpower_Ghn';
		$this->_headerText = __('GHN Order');
		$this->_addButtonLabel = __('Create New Order');
		parent::_construct();
	}
}
```
### **2.5** Create Collection (Important) to Grid convert data 
* **\app\code\[Vendor]\[Extention]\Model\GhnOrder.php [**3**]**

## **3** Create Collection : 
# IV.Front-end 
## 
# V.Back-end
## 1.Install Database or Schema
### **1.1** Create folder Setup 
* **app/code/[Vendor]/[Extention]/Setup**

### **1.2** Insert Data into Database 
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
* Go into Database. Search **setup_module** then DROP it. 
* Go to Server : **php bin/magento setup:upgrade**

## 2.Cron Job 
```bash
$ CRON JOB
. _______________________________________________________________________
├── etc
│     └── crontab.xml	
├── Cron
│     └── FileName.php
. ________________________________________________________________________
```
### **2.1** Create \app\code\[Vendor]\[Extention]\etc\crontab.xml
```xml
<?xml version="1.0" ?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/crontab.xsd">
	<group id="default">
		<job instance="[Vendor]\[Extention]\Cron\FileName" method="execute" name="netpower_ghn_cron">
			<schedule>* * * * *</schedule>
		</job>
	</group>
</config>
```

### **2.2** Create file Cron : \app\code\[Vendor]\[Extention]\Cron\FileName.php
```php
<?php
namespace [Vendor]\[Extention]\Cron;

class Test 
{
   public function __construct() 
    {}
 
   public function execute()
   {
      //CODE HERE
   }
}
```

### **2.3** Create connect Cron on server : 
```bash
$ crontab -e
```

```txt
#~ MAGENTO START
* * * * * /usr/bin/php /var/www/html/[Source Magento]/bin/magento cron:run | grep -v Ran jobs by schedule >> /var/www/html/[Source Magento]/var/log/magento.cron.log
* * * * * /usr/bin/php /var/www/html/[Source Magento]/update/cron.php >> /var/www/html/[Source Magento]/var/log/update.cron.log
* * * * * /usr/bin/php /var/www/html/[Source Magento]/bin/magento setup:cron:run >> /var/www/html/[Source Magento]/var/log/setup.cron.log
#~ MAGENTO END:

```
