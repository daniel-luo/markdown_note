##Complete##

```csv
sku,store_view_code,attribute_set_code,product_type,categories,product_websites,name,description,short_description,weight,product_online,tax_class_name,visibility,price,special_price,special_price_from_date,special_price_to_date,url_key,meta_title,meta_keywords,meta_description,base_image,base_image_label,small_image,small_image_label,thumbnail_image,thumbnail_image_label,swatch_image,swatch_image_label,created_at,updated_at,new_from_date,new_to_date,display_product_options_in,map_price,msrp_price,map_enabled,gift_message_available,custom_design,custom_design_from,custom_design_to,custom_layout_update,page_layout,product_options_container,msrp_display_actual_price_type,country_of_manufacture,additional_attributes,qty,out_of_stock_qty,use_config_min_qty,is_qty_decimal,allow_backorders,use_config_backorders,min_cart_qty,use_config_min_sale_qty,max_cart_qty,use_config_max_sale_qty,is_in_stock,notify_on_stock_below,use_config_notify_stock_qty,manage_stock,use_config_manage_stock,use_config_qty_increments,qty_increments,use_config_enable_qty_inc,enable_qty_increments,is_decimal_divided,website_id,related_skus,crosssell_skus,upsell_skus,additional_images,additional_image_labels,hide_from_product_page,bundle_price_type,bundle_sku_type,bundle_price_view,bundle_weight_type,bundle_values,configurable_variations,configurable_variation_labels,associated_skus
shirt-black,,Default,simple,Default Category/Gear/Bags,base,T-Shirt - Black,,,1,1,Taxable Goods,Not Visible Individually,1,,,,,,,,,,,,,,,,1/23/2016 19:45,1/23/2016 19:45,,,Block after Info Column,,,,,,,,,,,,,color=Black,5,0,1,0,0,1,1,1,0,1,1,,1,0,0,1,1,0,0,0,1,,,,,,,,,,,,,,
shirt-red,,Default,simple,Default Category/Gear/Bags,base,T-Shirt - Red,,,1,1,Taxable Goods,Not Visible Individually,1,,,,,,,,,,,,,,,,1/23/2016 19:45,1/23/2016 19:45,,,Block after Info Column,,,,,,,,,,,,,color=Red,5,0,1,0,0,1,1,1,0,1,1,,1,0,0,1,1,0,0,0,1,,,,,,,,,,,,,,
shirt,,Default,configurable,Default Category/Gear/Bags,base,T-Shirt,,,1,1,Taxable Goods,"Catalog, Search",,,,,,,,,,,,,,,,,1/23/2016 19:45,1/23/2016 19:45,,,Block after Info Column,,,,,,,,,,,,,size=55 cm,0,0,1,0,0,1,1,1,0,1,1,,1,1,1,1,1,0,0,0,1,,,,,,,,,,,,"sku=shirt-black,color=Black|sku=shirt-red,color=Red",color=Color,
```

##Required Field##
**如果是新产品** `product_websites` 为 `base` 是必须的

```csv
"sku","attribute_set_code","product_type","categories","product_websites","name","description","short_description","price","url_key","meta_title","meta_keywords","meta_description","additional_attributes","qty","is_in_stock","configurable_variations","configurable_variation_labels","associated_skus"
"Shirt-green-27","Default","simple","Default Category/Gear/Bags","base","T-Shirt - green",,,0,"Shirt-green-27",,,,"color=Apple",5,1,,,
```

##Gallery Import##
**position 是不生效的 M2 的问题**

```csv
sku,_media_image,_media_attribute_id,_media_is_disabled,_media_position,image,small_image,thumbnail
GC_900-PE0M,900-PE0M_01-BLACK_A_2000PX.jpg,87,1,1,900-PE0M_01-BLACK_A_2000PX.jpg,900-PE0M_01-BLACK_A_2000PX.jpg,900-PE0M_01-BLACK_A_2000PX.jpg
GC_900-PE0M,900-PE0M_01-BLACK_B_2000PX.jpg,87,0,2,,,
GC_900-PE0M,900-PE0M_01-BLACK_S_2000PX.jpg,87,0,3,,,
GC_900-PE0M,900-PE0M_02-NAVY_A_2000PX.jpg,87,0,4,,,
```

##Documetns##
[https://www.lexiconn.com/blog/2016/01/magento-2-import-configurable-products/](https://www.lexiconn.com/blog/2016/01/magento-2-import-configurable-products/)