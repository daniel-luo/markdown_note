##Solr大量数据索引刷新V2
Solr索引结构

**Simple Product**
```json
{
  "pid": "productId_storeId",
  "product_id": "entity_id",
  "sku": "product sku",
  "price": "min_price",
  "special_price": "special_price",
  "categories": [
    1,
    2,
    3
  ],
  "store_id": "store_id",
  "name": "product name",
  "type_id": "simple",
  "color": "option id",
  "color_t": "option_text"
}
```

**Configurable Prodeuct**
```json
{
  "pid": "productId_storeId",
  "product_id": "entity_id",
  "sku": "product sku",
  "price": "min_price",
  "special_price": "special_price",
  "categories": [
    1,
    2,
    3
  ],
  "store_id": "store_id",
  "name": "product name",
  "type_id": "configurable",
  "children_skus": [
    "sku1",
    "sku2",
    "sku3",
    "sku4"
  ],
  "children_color": [
    1,
    2,
    3
  ],
  "children_color_text": [
    "red",
    "yellow",
    "green"
  ]
}
```