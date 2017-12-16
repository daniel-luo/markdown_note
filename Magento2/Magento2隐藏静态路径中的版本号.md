##Magento2隐藏静态路径中的版本号
**eg:**
`[http://domain]/pub/static/version1474443317/frontend/Magento/luma/en_US/mage/calendar.css`
**version1474443317**
去后台关掉就不会显示了：
`Stores>Configuration>Advanced>Developer>Sign Static Files (dev_static_sign).`

- - -
相关类
**vendor/magento/module-theme/Model/Url/Plugin/Signature.php**

获取版本号
```php
/**
 * @var \Magento\Framework\App\View\Deployment\Version
 */
private $deploymentVersion;

$this->deploymentVersion->getValue()
```
版本号模板
```php
/**
 * Template of signature component of URL, parametrized with the deployment version of static files
 */
const SIGNATURE_TEMPLATE = 'version%s';
```
是否在路径中开启版本号
```config
Stores>Configuration>Advanced>Developer>Sign Static Files (dev_static_sign).
```
```php
/**
 * XPath for configuration setting of signing static files
 */
const XML_PATH_STATIC_FILE_SIGNATURE = 'dev/static/sign';
```
