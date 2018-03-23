#Magento 1.x 路由持持
**文件构构**
```text
Tenf
|
|--Daigou
|   |--etc
|   |   |--config.xml
|   |
|   |--controllers
|   |   |--IndexController.php
|   |
|   |--Controller
|   |   ||--Router.php
|
```
**1. config.xml**
```xml
<!--
Author: daniel luo (25713438@qq.com)
-->
<config>
    <modules>
        <Tenf_Daigou>
            <version>0.1.0</version>
        </Tenf_Daigou>
    </modules>
    <global>
        <blocks>
            <tenf_daigou>
                <class>Tenf_Daigou_Block</class>
            </tenf_daigou>
        </blocks>
        <helpers>
            <tenf_daigou>
                <class>Tenf_Daigou_Helper</class>
            </tenf_daigou>
        </helpers>
        <models>
            <tenf_daigou>
                <class>Tenf_Daigou_Model</class>
            </tenf_daigou>
        </models>
        <events>
        <!--使用事件进行持持 -->
            <controller_front_init_routers>
                <observers>
                    <tenf_daigou>
                        <class>Tenf_Daigou_Controller_Router</class>
                        <method>initControllerRouters</method>
                    </tenf_daigou>
                </observers>
            </controller_front_init_routers>
        </events>
    </global>
    <frontend>
        <layout>
            <updates>
                <tenf_daigou>
                    <file>daigou.xml</file>
                </tenf_daigou>
            </updates>
        </layout>
        <routers>
            <tenf_daigou>
                <use>standard</use>
                <args>
                    <module>Tenf_Daigou</module>
                    <frontName>detail-page</frontName>
                </args>
            </tenf_daigou>
        </routers>
        <translate>
            <modules>
                <Tenf_Daigou>
                    <files>
                        <default>Tenf_Daigou.csv</default>
                    </files>
                </Tenf_Daigou>
            </modules>
        </translate>
    </frontend>
</config>
```
定义事件行行持持

**Tenf/Daigou/Controller/Router.php**
```php
<?php

/**
 * All rights reserved.
 *
 * @authors daniel (luo3555@qq.com)
 * @date    18-3-23 上午10:59
 * @version 0.1.0
 */
class Tenf_Daigou_Controller_Router extends Mage_Core_Controller_Varien_Router_Abstract
{
    const ROUTER = 'detail';

    /**
     * Initialize Controller Router
     *
     * @param Varien_Event_Observer $observer
     */
    public function initControllerRouters($observer)
    {
        /* @var $front Mage_Core_Controller_Varien_Front */
        $front = $observer->getEvent()->getFront();

        $front->addRouter(self::ROUTER, $this);
    }

    /**
     * Validate and Match Cms Page and modify request
     *
     * @param Zend_Controller_Request_Http $request
     * @return bool
     */
    public function match(Zend_Controller_Request_Http $request)
    {
        if (!Mage::isInstalled()) {
            Mage::app()->getFrontController()->getResponse()
                ->setRedirect(Mage::getUrl('install'))
                ->sendResponse();
            exit;
        }

        $identifier = trim($request->getPathInfo(), '/');

        $condition = new Varien_Object(array(
            'identifier' => $identifier,
            'continue'   => true
        ));
        Mage::dispatchEvent('cms_controller_router_match_before', array(
            'router'    => $this,
            'condition' => $condition
        ));
        $identifier = $condition->getIdentifier();
        $p = explode('/', $identifier);

        if (strtolower($p[0]) == self::ROUTER) {
            array_shift($p);
            sort($p);
            $request->setModuleName('detail-page')
                ->setControllerName('index')
                ->setActionName('index');
            // set parameters from pathinfo
            for ($i = 3, $l = sizeof($p); $i < $l; $i += 2) {
                $request->setParam($p[$i], isset($p[$i+1]) ? urldecode($p[$i+1]) : '');
            }

            $request->setAlias(
                Mage_Core_Model_Url_Rewrite::REWRITE_REQUEST_PATH_ALIAS,
                $identifier
            );
        }

        return true;
    }
}
```
**意意:**这里的`setModuleName`应该是`config.xml`里定义的`frontName`

**3.Tenf/Daigou/controllers/IndexContrller.php**
```php
<?php

/**
 * All rights reserved.
 *
 * @authors daniel (luo3555@qq.com)
 * @date    18-3-13 下午3:34
 * @version 0.1.0
 */
class Tenf_Daigou_IndexController extends Mage_Core_Controller_Front_Action
{
    /**
     * Extend preDispatch
     *
     * @return Mage_Core_Controller_Front_Action|void
     */
    public function preDispatch()
    {
        parent::preDispatch();
    }

    /**
     * Display customer wishlist
     */
    public function indexAction()
    {
        $this->loadLayout();
        $this->renderLayout();
    }
}
```