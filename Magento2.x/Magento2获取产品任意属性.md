##Magento2获取产品任意属性
**1**
```php
$_product = $block->getProduct(); //get product object
$attribute_code="description"; // attribute with text value
$description_attribute = $_product->getResource()->getAttribute($attribute_code);
$description_attribute_value = $description_attribute ->getFrontend()->getValue($_product);
```

**2**
```php
$_product = $block->getProduct(); //get product object
$attribute_code="description"; // attribute with text value
 
// return attribute value,  will return option id instead of value for attribute with options
echo $_product->getData($attribute_code);
 
// return attribute value, will return option id instead of value for attribute with options
echo $_product->getResource()->getAttributeRawValue($_product->getId(),$attribute_code,  $_product->getStoreId());
 
$attribute_code="color"; //attribute with options
// return the thing value of attribute with options, if it’s a multiple select options, will return a array of value of selected options
var_dump($_product->getAttributeText($attribute_code));
 
// get product attribute label
var_dump($_product->getResource()->getAttribute($attribute_code)->getDefaultFrontendLabel());
var_dump($_product->getResource()->getAttribute($attribute_code)->getStoreLabel());
 
// get attribute input type,
var_dump($_product->getResource()->getAttribute($attribute_code)->getFrontendInput());
 
 
//Output attribute value for multiselect,select,boolean,text,textarea
if($_product->getResource()->getAttribute($attribute_code)){ //check attribute exist, return an object or null
    $attribute_type = $_product->getResource()->getAttribute($attribute_code)->getFrontendInput();
    $attribute_value = "";
    if ($attribute_type == "text" || $attribute_type == "textarea") {
        $attribute_value = $_product->getResource()->getAttributeRawValue($_product->getId(), $attribute_code, $_product->getStoreId());
    }elseif ($attribute_type == "multiselect" || $attribute_type == "select" || $attribute_type == "boolean") {
        $attribute_value = $_product->getAttributeText($attribute_code);
        // for multiple select attribute
        if (count($attribute_value) > 1) {
            $attribute_value = implode(", ", $attribute_value);
        }
    }else{
        //other attribute type
        $attribute_value = "";
    }
    echo $attribute_value;
}
```
**3**
```php
$attribute = $this->eavConfig->getAttribute('catalog_product', 'attribute_code_here');
$options = $attribute->getSource()->getAllOptions();
```
