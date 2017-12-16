##Magento2响应页面

```php
/** @var \Magento\Framework\Controller\ResultInterface $response */
$response = $this->resultFactory->create('json'); // page, raw, redirect, forward, layout
$response->setHeader('Content-Type', 'application/json', true);
$response->setData($result);
return $response;
```
