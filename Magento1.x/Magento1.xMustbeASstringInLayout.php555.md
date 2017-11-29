Fatal error: Uncaught Error: Function name must be a string in ... **app\code\core\Mage\Core\Model\Layout.php:555**

This error was easy to fix because the problem was in the following line:

`$out .= $this->getBlock($callback[0])->$callback[1]();`

Instead it should be:

`$out .= $this->getBlock($callback[0])->{$callback[1]}();`