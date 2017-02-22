##Magento2 发布步骤##
1. `sudo rm -fr pub/static`
2. `sudo mkdir -p pub/static`
3. `sudo chmod -R 777 pub/static`
4. `sudo php bin/magento setup:upgrade`
5. `sudo chmod -R 777 var`
6. `sudo php bin/magento setup:static-content:deploy`
7. `sudo chmod -R 777 var`
8. `sudo php bin/magento cache:clean`