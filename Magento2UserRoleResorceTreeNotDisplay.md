##Magento2.1 后台 Role Resource Tree 不显示##
**原因:** Resource Tree 的排序被打乱了
在 `/vendor/magento/module-user/Block/Role/Tab/Edit.php` 中 `public function getTree()` 能获取到所有的 `$resources` 打印出来后正确结构如下， `0` 和 `1` 允许所有权限和所有权限列表, 参考 `getTree()` 发下有如下语句 `isset($resources[1]['children']) ? $resources[1]['children'] : []`， 如果资源树顺序被打乱，则无法获取到对应的支援树，后端将无法显示资源树。

**解决办法:**
在 `/vendor/magento/module-user/Block/Role/Tab/Edit.php` 的 `public function getTree()` 将资源树打印出来，　确保 `0` 和 `1` 是 `Magento_Backend::all` 和 `Magento_Backend::admin`, 如果不是则全局搜索该 `id` 然后给这个对应的 `resoruce` 设置 `sortOrder`, 自定义的 `sortOrder` 尽量大于20, 如果不设置 `sortOrder` 默认为 `0`!

```php
Array
(
    [0] => Array
        (
            [id] => Magento_Backend::all
            [title] => Allow everything
            [sortOrder] => 10
            [children] => Array
                (
                )

        )

    [1] => Array
        (
            [id] => Magento_Backend::admin
            [title] => Magento Admin
            [sortOrder] => 20
            [children] => Array
                (
                    [0] => Array
                        (
                            [id] => Magento_Backend::dashboard
                            [title] => Dashboard
                            [sortOrder] => 0
                            [children] => Array
                                (
                                )

                        )
```

**获取资源树的方法**
`/vendor/magento/module-user/Block/Role/Tab/Edit.php`

```php
    /**
     * Get Json Representation of Resource Tree
     *
     * @return array
     */
    public function getTree()
    {
        $resources = $this->_aclResourceProvider->getAclResources();
        $rootArray = $this->_integrationData->mapResources(
            isset($resources[1]['children']) ? $resources[1]['children'] : []
        );
        return $rootArray;
    }
```

###错误的资源定义###
**以下的资源定义是不被允许的**
缺少　`sortOrder`, `sortOrder` 必须被定义且大于 **20**

```xml
 <resource id="Emizen_Ajaxscroll::UNKNOWN_all" title="Allow Everything"/>
```

```xml
<resource id="Magento_Adminhtml::admin">
```

###正确的资源定义###
必须定义 `sortOrder` 且值应该大于 **20**

```xml
<resource id="Emizen_Ajaxscroll::UNKNOWN_all" title="Allow Everything" sortOrder="100"/>
```