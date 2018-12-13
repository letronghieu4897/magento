![Mọi](https://image.flaticon.com/icons/svg/818/818080.svg) 
![Beginner](https://img.shields.io/badge/Magento-Beginner-brightgreen.svg)
![Server](https://img.shields.io/badge/Magento-CetOS-red.svg)
![System](https://img.shields.io/badge/Magento-System-brightgreen.svg)
![Admin Site](https://img.shields.io/badge/Magento-Admin-red.svg)
![Front-end](https://img.shields.io/badge/Magento-Frontend-brightgreen.svg)
![Back-end](https://img.shields.io/badge/Magento-Backend-red.svg)

# TABLE CONTENTs
### I. Linux Server
 - **1**. Change user
 - **2**. Multiple site 
 - **3**. Command support
 - **4**. Patch Security Upgrade Version 
 
### II. System
- **1**. Set mode
- **2**. Clear cache
- **3**. Indexer

### III. Admin Site
- **1**. Create new menu
- **2**. Create Grid on admin
- **3**. Create new field 
	- **3.1**. Front-end : Show field
	- **3.2**. Back-end : source data for field
- **4**. Create Config for Cron Job
	- **4.1**. Create Cron Group
	- **4.2**. Load Extention for managing CRON JOB
### IV. Front-end
- **1**. Override Order Confirmation Email
- **2**. Override block, theme.........
- **3**. Create new tab

### V. Back-end
- **1**. Install Database or Schema
- **2**. Cron Job 
- **3**. Collection
	- **3.1**. Create Collection
	- **3.2**. Using Factory
- **4**. Get Data 
	- **4.1**. From Admin Field
	
### VI. Windows
- **1**. XAMPP

 **********

# I. Linux Server
## **1**. Change user 
* **chown -R [username].[groupname] /[path]** : chown -R hieunetpower.hieunetpower /var/www/
## **2**. Multiple site 
* create file in : **/etc/nginx/sites-enabled/[name].conf**
```conf
server {
     listen 80;
     server_name  scanholm.local www.scanholm.local;

     set $MAGE_ROOT /var/www/scanholm; [path of source]
     set $MAGE_MODE developer;
     set $MAGE_RUN_TYPE website;
     include /var/www/scanholm/nginx.conf.sample;
}
```
* **setup env.php**
```php
<?php
Copy from env.php.local
```
 * **setup upgrade**
 * **enable cache**
 
 ## **3**. Command support 
 ```bash
 1. Find in folder
$ grep -rn "Word_to_need_find" [Folder]/
 ```
 
 ## **4**. Patch Security Upgrade Version
 ```txt
 . _______________________________________________________________________
├── app
|    ├── etc
│    │ 	  ├── local.xml ---------------	[Create new file]	
│    │ 	  └── local.xml.staging -------	[Copy and Paste to local.xml]	
|    └── Mage.php ---------------------	[Check version patch security]
├── var
|    └── cache ------------------------ [Clean cache]
._________________________________________________________________________
 
 1. Download newest version patch security
 2. app > etc > local.xml.staging [Copy]
 3. [Paste] Create new file local.xml in etc.
 4. Check version of Patch Security : Go to file Mage.php 
 5. Copy file patch downloaded to root of source.
 6. Run sh [file].sh
 7. Delete folder cache : Clear cache
 8. Testing Code : Show diff 
 9. Testing Page : Order, Payment, Login, ...
 ```
 ```php
 Mage.php 
 
 PATCH IN HERE : 1901--
 public static function getVersionInfo()
    {
        return array(
            'major'     => '1',
            'minor'     => '9',
            'revision'  => '0',
            'patch'     => '1',
            'stability' => '',
            'number'    => '',
        );
    }
 ```
 **********
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
 **********
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
* **\app\code\[Vendor]\[Extention]\Model\GhnOrder.php [**V [3]**]**

## **3** Create new field 
```bash
$ NEW FIELD ON ADMIN and set data 
. _____________________________________________________________________________
├── adminhtml
│     └── system.xml				: Create new field on admin
├── Model
│     └── Config
|	     └── Source
|		    └── source.php		: Set data for field (Option)
. _____________________________________________________________________________
```
![adminlayout](https://cdn.mageplaza.com/media/general/P8E2i4k.png) 

### **3.1**. Front-end : Show field 
* **\app\code\[Vendor]\[Extention]\etc\adminhtml\system.xml**
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Config:etc/system_file.xsd">
    <system>
        <section id="carriers" translate="label" type="text" sortOrder="320" showInDefault="1" showInWebsite="1" showInStore="1">
            <group id="giaohangnhanh" translate="label" type="text" sortOrder="0" showInDefault="1" showInWebsite="1" showInStore="1">
                <label>GHN</label>
                <field id="token_ghn" translate="label" type="text" sortOrder="1" showInDefault="1" showInWebsite="1" showInStore="1">
                    <label>Token</label>
                </field>
                <field id="active" translate="label" type="select" sortOrder="3" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Enabled</label>
                    <source_model>Magento\Config\Model\Config\Source\Yesno</source_model>
                </field>
                <field id="name" translate="label" type="text" sortOrder="4" showInDefault="1" showInWebsite="1" showInStore="1">
                    <label>Method Name</label>
                </field>
                <field id="price" translate="label" type="text" sortOrder="5" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Price</label>
                    <validate>validate-number validate-zero-or-greater</validate>
                </field>
                <field id="handling_type" translate="label" type="select" sortOrder="7" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Calculate Handling Fee</label>
                    <source_model>Magento\Shipping\Model\Source\HandlingType</source_model>
                </field>
                <field id="handling_fee" translate="label" type="text" sortOrder="8" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Handling Fee</label>
                    <validate>validate-number validate-zero-or-greater</validate>
                </field>
                <field id="sort_order" translate="label" type="text" sortOrder="100" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Sort Order</label>
                </field>
                <field id="title" translate="label" type="text" sortOrder="2" showInDefault="1" showInWebsite="1" showInStore="1">
                    <label>Title</label>
                </field>
                <field id="sallowspecific" translate="label" type="select" sortOrder="90" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Ship to Applicable Countries</label>
                    <frontend_class>shipping-applicable-country</frontend_class>
                    <source_model>Magento\Shipping\Model\Config\Source\Allspecificcountries</source_model>
                </field>
                <field id="specificcountry" translate="label" type="multiselect" sortOrder="91" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Ship to Specific Countries</label>
                    <source_model>Magento\Directory\Model\Config\Source\Country</source_model>
                    <can_be_empty>1</can_be_empty>
                </field>
                <field id="showmethod" translate="label" type="select" sortOrder="92" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Show Method if Not Applicable</label>
                    <source_model>Magento\Config\Model\Config\Source\Yesno</source_model>
                </field>
                <field id="specificerrmsg" translate="label" type="textarea" sortOrder="80" showInDefault="1" showInWebsite="1" showInStore="1">
                    <label>Displayed Error Message</label>
                </field>
                <field id="fromdistrictid" translate="label" type="select" sortOrder="101" showInDefault="1443" showInWebsite="1443" showInStore="1443">
                    <label>Place of Warehouse</label>
                    <source_model>Netpower\Ghn\Model\Config\Source\WareHouseGhn</source_model>
                </field>
                <field id="sync_district" translate="label comment tooltip" type="button" sortOrder="120" showInDefault="1" showInWebsite="1" showInStore="0">
                    <label>Synchronize Province - District </label>
                <frontend_model>Netpower\Ghn\Block\System\Config\Button</frontend_model>
            </field>
            </group>
        </section>
    </system>
</config>
```

### **3.2**. Back-end : source data for field 
```xml
Use it : <source_model>Netpower\Ghn\Model\Config\Source\[name]</source_model>
```
* **\app\code\[Vendor]\[Extention]\Model\Config\Source\[name].php**
```php
<?php 

namespace Netpower\Ghn\Model\Config\Source;

use Netpower\Ghn\Helper\GhnApi;
use Netpower\Ghn\Services\Config;

class [name] implements \Magento\Framework\Option\ArrayInterface
{      

    public function __construct() {
    }
    /**
     * Options warehouse of Ghn
     *
     * @return array
     */
    public function toOptionArray()
    {   
     
        $result = array();

         $result[] = [
                            'value' => 100,
                            'label' => "One Hundred"
                       ];
        }
        return $result;
    }
}
```
## **4**. Create Config for Cron Job
```php
Data will be stored in table core_config_data
```
```bash
$ CRON JOB CONFIG

. _______________________________________________________________________
├── etc
│     ├── crontab.xml		 	: Define Job and Group for Cron
|     ├── cron_groups.xml	 	: Create new Cron group
|     ├── config.xml		 	: Set default value
|     └── adminhtml		
|	     └── system.xml		: Show config admin
├── Cron
│     └── FileName.php		 	: Execute logic
├── Model
|     └── Config
|	     └── Backend 
|		    └── Cron.php 	: Store Database
. ________________________________________________________________________
```
* **\app\code\[Vendor]\[Extention]\etc\crontab.xml**
```xml 
<?xml version="1.0" ?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/crontab.xsd">
	<group id="ghn">
		<job instance="Netpower\Ghn\Cron\SyncOrder" method="execute" name="netpower_ghn_cron">
			<config_path>crontab/[ID CRON GROUP]/jobs/[NAME OF JOB]/schedule/cron_expr</config_path>
		</job>
	</group>
</config>
```
**\app\code\[Vendor]\[Extention]\etc\config.xml**
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Store:etc/config.xsd">
    <default>
        <crontab>
            <ghn>
                <jobs>
                    <netpower_ghn_cron>
                        <schedule>
                            <cron_expr><![CDATA[0 1 * * *]]></cron_expr>
                        </schedule>
                    </netpower_ghn_cron>
                </jobs>
            </ghn>
        </crontab>
    </default>
</config>
```
* **\app\code\[Vendor]\[Extention]\etc\system.xml**
```xml
		<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Config:etc/system_file.xsd">
    <system>
        <section id="carriers" translate="label" type="text" sortOrder="320" showInDefault="1" showInWebsite="1" showInStore="1">
            <group id="giaohangnhanh" translate="label" type="text" sortOrder="0" showInDefault="1" showInWebsite="1" showInStore="1">
                <label>GHN</label>
             	 <field id="sync_district_cron" type="text"  translate="label comment tooltip" sortOrder="10" showInDefault="1">
                    <label>Schedule Update Status GHN</label>  
                    <validate>required-entry</validate>
                    <attribute type="cron"><![CDATA[netpower_ghn_cron]]></attribute>
                    <attribute type="group"><![CDATA[ghn]]></attribute>
                    <backend_model>Netpower\Ghn\Model\Config\Backend\Cron</backend_model>
                    <comment>
                        <![CDATA[For instance 0 1 * * * !]]>
                    </comment>
                </field>
            </group>
        </section>
    </system>
</config>
```
### **4.1**. Create Cron Group 
* **app/code/[Vendor]/[Extention]/etc/cron_groups.xml**
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/cron_groups.xsd">
    <group id="ghn">
        <schedule_generate_every>2</schedule_generate_every>
        <schedule_ahead_for>8</schedule_ahead_for>
        <schedule_lifetime>4</schedule_lifetime>
        <history_cleanup_every>10</history_cleanup_every>
        <history_success_lifetime>120</history_success_lifetime>
        <history_failure_lifetime>1200</history_failure_lifetime>
        <use_separate_process>2</use_separate_process>
    </group>
</config>
```
### **4.2**. Load Extention for managing CRON JOB
```bash
$ composer require ethanyehuda/magento2-cronjobmanager
```
### **4.3**. Create Backend Model for Cron Job 
```php
To save data into database in core_config_data table
```
```php
<?php

namespace Netpower\NERPIs\Model\Config\Backend;

class Cron extends \Magento\Framework\App\Config\Value
{
    /**
     * Cron string path
     */
    const CRON_STRING_PATH = 'crontab/%s/jobs/%s/schedule/cron_expr';

    /**
     * @var \Magento\Framework\App\Config\ValueFactory
     */
    protected $_configValueFactory;

    /**
     * @param \Magento\Framework\Model\Context $context
     * @param \Magento\Framework\Registry $registry
     * @param \Magento\Framework\App\Config\ScopeConfigInterface $config
     * @param \Magento\Framework\App\Cache\TypeListInterface $cacheTypeList
     * @param \Magento\Framework\App\Config\ValueFactory $configValueFactory
     * @param \Magento\Framework\Model\ResourceModel\AbstractResource $resource
     * @param \Magento\Framework\Data\Collection\AbstractDb $resourceCollection
     * @param string $runModelPath
     * @param array $data
     */
    public function __construct(
        \Magento\Framework\Model\Context $context,
        \Magento\Framework\Registry $registry,
        \Magento\Framework\App\Config\ScopeConfigInterface $config,
        \Magento\Framework\App\Cache\TypeListInterface $cacheTypeList,
        \Magento\Framework\App\Config\ValueFactory $configValueFactory,
        \Magento\Framework\Model\ResourceModel\AbstractResource $resource = null,
        \Magento\Framework\Data\Collection\AbstractDb $resourceCollection = null,
        $runModelPath = '',
        array $data = []
    ) {
        $this->_configValueFactory = $configValueFactory;
        parent::__construct($context, $registry, $config, $cacheTypeList, $resource, $resourceCollection, $data);
    }

    public function beforeSave()
    {
        $label = $this->getData('field_config/label'); 			//Get Text between <label> TEXT </label>
        $cronValue = $this->getValue();
        $messageError = $this->validateCron($cronValue);
        
        if ($messageError) {
            throw new \Magento\Framework\Exception\ValidatorException($messageError);
        }

        parent::beforeSave();
    }

    protected function validateCron($expr)
    {
        $error = "";
        $e = preg_split('#\s+#', $expr, null, PREG_SPLIT_NO_EMPTY);

        if (sizeof($e)<5 || sizeof($e)>6) {
            $error = __('Invalid cron expression: '. $expr);
        }

        return $error;
    }

    /**
     * {@inheritdoc}
     *
     * @return $this
     * @throws \Exception
     */
    public function afterSave()
    {
        $cronName = $this->getData('field_config/cron');       //Get Text between <attribute type="N"> TEXT </attribute> with Type cron
        $cronGroup = $this->getData('field_config/group');
        $cronValue = $this->getValue();
        $cronPath = sprintf(self::CRON_STRING_PATH, $cronGroup, $cronName);

        try {
            $this->_configValueFactory->create()->load(
                $cronPath,
                'path'
            )->setValue(
                $cronValue
            )->setPath(
                $cronPath
            )->save();
        } catch (\Exception $e) {
            throw new \Exception(__('We can\'t save the cron expression.'));
        }

        return parent::afterSave();
    }
}
```

 **********
# IV.Front-end 
## 1.Override Order Confirmation Email
```txt
Marketing > Email Template > Add New Template > Choose New Order > Load
```
```bash
$ OVERRIDE ORDER CONFIRMATION EMAIL
. ________________________________________________________________________________________
├── design
│     └── frontend
|	     └── [Vendor]
|		     └── [Extention]
|			      └── Magento_Sales
|					 └── templates
|						 └── email
|							└── items
|							      └── order
|								     └── default.phtml
. _________________________________________________________________________________________
```
```txt 
Go to registration.php 
\vendor\magento\module-sales\registration.php
```
```php
<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::MODULE,
    'Magento_Sales',
    __DIR__
);

```
```txt
Create folder in 

├── design
│     └── frontend
|	     └── [Vendor]
|		     └── [Extention]

With name is Magento_Sales : get from registration.php

Then create path source same path into file default.phtml [Look at Tree]
```
## 2.Override block, theme.........
```php
1. Go into file registration.php : -> get name of folder : [Netpower_Ghn]
2. Go to design > frontend > [Vendor] > [Theme] > Create folder with name get from [1].
3. Create file name the same with file is overrided [Same path]
```

```xml
Block 
<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <update handle="review_product_form_component"/>
    <body>
        <referenceBlock name="product.info.details">
            <referenceBlock name="reviews.tab" remove="true"/>
             <block class="Magento\Review\Block\Product\Review" name="trustpilot.review.tab" template="Trustpilot_Reviews::review.phtml" />
        </referenceBlock>
    </body>
</page>


1. referenceBlock : Only need the name. [Or referenceContainer]
2. Only reference container contain the block we want to override. 
```
## 3.Create new tab 
```xml
<Example> Review Product </Example>
. ___________________________________________
├── [Vendor]_[Extention]
│            ├── layout
|	     │     └── catalog_product_view.xml
|	     └── templates 
|		   └── review.phtml
. ____________________________________________

<1> Create theme </1>
<2> Create file same name in Core : catelog_product_view.xml</2>
<3> Create new block </3>
<4> Create template file : templates > review.phtml
```
```xml
catalog_product_view.xml

<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceBlock name="product.info.details">
            <referenceBlock name="reviews.tab" remove="true"/>
            <block class="Magento\Catalog\Block\Product\View" name="trustpilot.review.tab" as="trustpilot.reviews" template="Trustpilot_Reviews::review.phtml" group="detailed_info">
            	<arguments>
            		<argument translate="true" name="title" xsi:type="string">Review</argument>
            	</arguments>
            </block>
        </referenceBlock>
    </body>
</page>
```
```phtml
$block : this variable use as Class in block declare above. [Magento\Catalog\Block\Product\View] 

We can you $block as View. 
```

 **********
 * 
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

```txt
* * * * * *
| | | | | | 
| | | | | +-- Year              (range: 1900-3000)
| | | | +---- Day of the Week   (range: 1-7, 1 standing for Monday)
| | | +------ Month of the Year (range: 1-12)
| | +-------- Day of the Month  (range: 1-31)
| +---------- Hour              (range: 0-23)
+------------ Minute            (range: 0-59)
```

```txt
* * * * * *                         Each minute


59 23 31 12 5 *                     One minute  before the end of year if the last day of the year is Friday
									
59 23 31 DEC Fri *                  Same as above (different notation)


45 17 7 6 * *                       Every  year, on June 7th at 17:45


45 17 7 6 * 2001,2002               Once a   year, on June 7th at 17:45, if the year is 2001 or  2002


0,15,30,45 0,6,12,18 1,15,31 * 1-5 *  At 00:00, 00:15, 00:30, 00:45, 06:00, 06:15, 06:30,
                                    06:45, 12:00, 12:15, 12:30, 12:45, 18:00, 18:15,
                                    18:30, 18:45, on 1st, 15th or  31st of each  month, but not on weekends


*/15 */6 1,15,31 * 1-5 *            Same as above (different notation)


0 12 * * 1-5 * (0 12 * * Mon-Fri *) At midday on weekdays


* * * 1,3,5,7,9,11 * *              Each minute in January,  March,  May, July, September, and November


1,2,3,5,20-25,30-35,59 23 31 12 * * On the  last day of year, at 23:01, 23:02, 23:03, 23:05,
                                    23:20, 23:21, 23:22, 23:23, 23:24, 23:25, 23:30,
                                    23:31, 23:32, 23:33, 23:34, 23:35, 23:59


0 9 1-7 * 1 *                       First Monday of each month, at 9 a.m.


0 0 1 * * *                         At midnight, on the first day of each month


* 0-11 * * *                        Each minute before midday


* * * 1,2,3 * *                     Each minute in January, February or March


* * * Jan,Feb,Mar * *               Same as above (different notation)


0 0 * * * *                         Daily at midnight


0 0 * * 3 *                         Each Wednesday at midnight
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

### **2.4** Run Crontab
```bash
$ php bin/magento cache:flush
$ php bin/magento cron:run --group="default"

//"default" is group declared in crontab.xml
```

## **3** Collection 

### **3.1** Create Collection
```bash
$ COLLECTION
. _______________________________________________________________________
├── Model
|     ├── GhnOrder.php 				[1]
|     └── ResourceModel				
|		├── GhnOrder.php		[2]
|		└── GhnOrder 
|			└── Collection.php 	[3]
|
. ________________________________________________________________________
```
* **[1] \app\code\[Vendor]\[Extention]\Model\GhnOrder.php**
```php
<?php
namespace [Vendor]\[Extention]\Model;

use Magento\Framework\Model\AbstractModel;
	
	/**
	 * Collection to get data from directory_country_region
	 */
    class GhnOrder extends AbstractModel
    {   
        protected function _construct()
        {
            $this->_init('[Vendor]\[Extention]\Model\ResourceModel\GhnOrder');
        }
    }
```

* **[2] \app\code\[Vendor]\[Extention]\Model\ResourceModel\GhnOrder.php**
```php
<?php
namespace [Vendor]\[Extention]\Model\ResourceModel;
class GhnOrder extends \Magento\Framework\Model\ResourceModel\Db\AbstractDb
{
    /**
     * Define main table
     */
    protected function _construct()
    {
        $this->_init('sale_ghn_order', 'entity_id');   
	//here "sale_ghn_order" is table name and "entity_id" is the key
    }
}
```

* **[3] \app\code\[Vendor]\[Extention]\Model\ResourceModel\GhnOrder\Collection.php**
```php
<?php

/**
* Ecommerce Resource Collection
*/
namespace [Vendor]\[Extention]\Model\ResourceModel\GhnOrder;

class Collection extends \Magento\Framework\Model\ResourceModel\Db\Collection\AbstractCollection{
/**
* Resource initialization
*
* @return void
*/
protected function _construct(){
	$this->_init('[Vendor]\[Extention]\Model\GhnOrder', '[Vendor]\[Extention]\Model\ResourceModel\GhnOrder');
	}
}
```

### **3.2** Using Factory 
- **Example**
```php
<?php
namespace Netpower\Ghn\Controller\Adminhtml\System;

use \Magento\Catalog\Model\Product\Visibility;

use Netpower\Ghn\Helper\GhnApi;
use Netpower\Ghn\Model\GetRegionIdFactory;
use Netpower\Ghn\Services\Transport;

use Netpower\Ghn\Services\Config;

class Button extends \Magento\Backend\App\Action
{   
    protected $_ghnApi;
    protected $_getRegionId;
    protected $_log;

    public function __construct(
        \Magento\Backend\App\Action\Context $context,
        GhnApi $ghnApi,
        GetRegionIdFactory $getRegionId,
        Transport $log
         
    ) { 
        $this->_ghnApi = $ghnApi;
        $this->_getRegionId = $getRegionId;
        $this->_log = $log;
        parent::__construct($context);
    }
    public function execute(){

        $token = Config::TOKEN_STAGING;
        $requestToken = array(
            'token' => $token
        );
        
        $requestTokens = json_encode($requestToken);
        $listDistrictProvinceArray = $this->_ghnApi->getDistrictProvince($requestTokens);

	/**********************************************************************
	 *			HERE IS FACTORY
	 **********************************************************************/
        $districts = $this->_getRegionId->create()->getCollection()->getData();

        $this->_log->log($districts);

        }
}
```
- **Get Data**
```php
 $getData = $this->_getRegionId->create()->getCollection()->getData();

```
- **Set Data**
```php
 $setData = $this->_getRegionId->create();
 $setData->setData([name_of_column], $data);
 $setData->save()
```
- **Load Data**
```php
$loadData = $this->_ghnOrder->create()->load(['name_of_column']);
```
- **Update Data** 
```php
//Combine Set and Load Data

$loadData = $this->_ghnOrder->create()->load(['id_of_table']);

$loadData->setData([name_of_column], $data);
$loadData->save()

```
## **4**. Get Data 
### **4.1**. From Admin Field
```php
<?php
namespace Netpower\Ghn\Model;

use \Magento\Store\Model\ScopeInterface;

class Carrier extends AbstractCarrier implements CarrierInterface
{
/**
 * @var string
 */

/**
 * @param \Magento\Framework\App\Config\ScopeConfigInterface $scopeConfig
 * @param \Magento\Quote\Model\Quote\Address\RateResult\ErrorFactory $rateErrorFactory
 * @param \Psr\Log\LoggerInterface $logger
 * @param \Magento\Shipping\Model\Rate\ResultFactory $rateResultFactory
 * @param \Magento\Quote\Model\Quote\Address\RateResult\MethodFactory $rateMethodFactory
 * @param \Netpower\Ghn\Controller\Getdata\Getdata $fee
 * @param array $data
 */
public function __construct(
    ScopeConfigInterface $scopeConfig, 
    array $data = []
)
{
    parent::__construct($scopeConfig, $rateErrorFactory, $logger, $data);
}

/**
 * @return array
 */
public function getAllowedMethods()
{
    /*0*/
    return ['giaohangnhanh' => $this->getConfigData('name') ];
}

/**
 * Processing additional validation to check is carrier applicable.
 *
 * @param \Magento\Framework\DataObject $request
 * @return $this|bool|\Magento\Framework\DataObject
 * @SuppressWarnings(PHPMD.UnusedFormalParameter)
 */
public function processAdditionalValidation(DataObject $request)
{
    /*1*/
    return $this;
}

/**
 * @param RateRequest $request
 * @return bool|Result
 */
public function collectRates(RateRequest $request)
{
    $adminField = $this->_scopeConfig->getValue('[id of section]/[id of group]/[id of field]', ScopeInterface::SCOPE_STORE);
}

```

# VI.Windows
## 1.XAMPP
```php
Change in : xampp > apache > conf > extra > httpd-vhosts.conf
```
```conf
<VirtualHost *:88>
    ##ServerAdmin webmaster@dummy-host2.example.com
    DocumentRoot "C:/xampp/htdocs/netpower/magento/Magento_CE_1_9_Salgvest"
    ServerName salgvest.local
    ##ErrorLog "logs/netpower/magento/Magento_CE_1_9_Salgvest-error.log"
    ##CustomLog "logs/netpower/magento/Magento_CE_1_9_Salgvest-access.log" common
</VirtualHost>
```
```php
Change in : xampp > apache > conf > httpd.conf : chang port to 88
```
