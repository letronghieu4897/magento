![alt text](https://image.flaticon.com/icons/svg/818/818080.svg) 
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
# Front-end 
## 
# Back-end
## Install Database or Schema
* Create folder Setup : [Vendor]/[Extention]/Setup 
* Insert Data into Database 
  * Create file : **InstallData.php**
  * Create Schema or Insert New Column : **InstallSchema.php**
* 
