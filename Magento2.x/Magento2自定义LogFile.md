#Magento2 自定义 Log File
只能复写log模块添加 [详见](https://magento.stackexchange.com/questions/75935/how-to-create-custom-log-file-in-magento-2)

**1. Write Logger class in Logger/Logger.php:**
```php
namespace YourNamespace\YourModule\Logger;

class Logger extends \Monolog\Logger
{
}
```
**2. Write Handler class in Logger/Handler.php:**
```php
namespace YourNamespace\YourModule\Logger;

use Monolog\Logger;

class Handler extends \Magento\Framework\Logger\Handler\Base
{
    /**
     * Logging level
     * @var int
     */
    protected $loggerType = Logger::INFO;

    /**
     * File name
     * @var string
     */
    protected $fileName = '/var/log/myfilename.log';
}
```
**Note:** This is the only step which uses Magento code. \Magento\Framework\Logger\Handler\Base extends Monolog's StreamHandler and e.g. prepends the $fileName attribute with the Magento base path.

**3.Register Logger in Dependency Injection etc/di.xml:**
```php
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xsi:noNamespaceSchemaLocation="../../../../../lib/internal/Magento/Framework/ObjectManager/etc/config.xsd">
    <type name="YourNamespace\YourModule\Logger\Handler">
        <arguments>
            <argument name="filesystem" xsi:type="object">Magento\Framework\Filesystem\Driver\File</argument>
        </arguments>
    </type>
    <type name="YourNamespace\YourModule\Logger\Logger">
        <arguments>
            <argument name="name" xsi:type="string">myLoggerName</argument>
            <argument name="handlers"  xsi:type="array">
                <item name="system" xsi:type="object">YourNamespace\YourModule\Logger\Handler</item>
            </argument>
        </arguments>
    </type>
</config>
```
**Note:** This is not strictly required but allows the DI to pass specific arguments to the constructor. If you do not do this step, then you need to adjust the constructor to set the handler.

**4. Use the logger in your Magento classes:**
This is done by Dependency Injection. Below you will find a dummy class which only writes a log entry:

```php
namespace YourNamespace\YourModule\Model;

class MyModel
{
    /**
     * Logging instance
     * @var \YourNamespace\YourModule\Logger\Logger
     */
    protected $_logger;

    /**
     * Constructor
     * @param \YourNamespace\YourModule\Logger\Logger $logger
     */
    public function __construct(
        \YourNamespace\YourModule\Logger\Logger $logger
    ) {
        $this->_logger = $logger;
    }

    public function doSomething()
    {
        $this->_logger->info('I did something');
    }
}
```
* * *
**不同Log区别**
```php
namespace Mageplaza\HelloWorld\Post;
class Post extends \Magento\Framework\View\Element\Template
{
        protected $_logger;
 
    public function __construct(
        \Magento\Backend\Block\Template\Context $context,
        \Psr\Log\LoggerInterface $logger,
        array $data = []
    )
    {        
        $this->_logger = $logger;
        parent::__construct($context, $data);
    }
    
    public function testLogging() 
    {    
        // monolog's Logger class
        // MAGENTO_ROOT/vendor/monolog/monolog/src/Monolog/Logger.php
        
        // saved in var/log/debug.log
        $this->_logger->debug('debug1234'); 
        //Output: [2017-02-22 04:48:44] main.DEBUG: debug1234 {"is_exception":false} []
        
        $this->_logger->info('info1234'); 
        // Write to default log file: var/log/system.log
        //Output: [2017-02-22 04:52:56] main.INFO: info1234 [] []
        
        $this->_logger->alert('alert1234'); 
        // Write to default log file: var/log/system.log
        //Output: [2017-02-22 04:52:56] main.ALERT: alert1234 [] []
        
        $this->_logger->notice('notice1234'); 
        // Write to default log file: var/log/system.log
        //Output: [2017-02-22 04:52:56] main.NOTICE: notice1234 [] []
        
        // Write to default log file: var/log/system.log
        $this->_logger->error('error1234'); 
        //Output: [2017-02-22 04:52:56] main.ERROR: error1234 [] []
        
         // Write to default log file: var/log/system.log
        $this->_logger->critical('critical1234'); 
        //Output: [2017-02-22 04:52:56] main.CRITICAL: critical1234 [] []
        
        // Adds a log record at an arbitrary level
        $level = 'DEBUG';
        // saved in var/log/debug.log
        $this->_logger->log($level,'debuglog1234', array('msg'=>'123', 'new' => '456')); 
        //Output: [2017-02-22 04:52:56] main.DEBUG: debuglog1234 {"msg":"123","new":"456","is_exception":false} []
        
        // Write to default log file: var/log/system.log
        $level = 'ERROR';
        $this->_logger->log($level,'errorlog1234', array( array('test1'=>'123', 'test2' => '456'), array('a'=>'b') )); 
        //Output: [2017-02-22 04:52:56] main.ERROR: errorlog1234 [{"test1":"123","test2":"456"},{"a":"b"}] []        
        
    }
    
}
```

##Documents
[https://magento.stackexchange.com/questions/75935/how-to-create-custom-log-file-in-magento-2](https://magento.stackexchange.com/questions/75935/how-to-create-custom-log-file-in-magento-2)
[https://www.mageplaza.com/how-write-log-magento-2.html](https://www.mageplaza.com/how-write-log-magento-2.html)