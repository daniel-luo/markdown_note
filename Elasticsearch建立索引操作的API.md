##Elasticsearch建立索引操作的API##
1. **用_cat API检测集群是否健康。 确保9200端口号可用:**
`curl 'localhost:9200/_cat/health?v'`
绿色表示一切正常, 黄色表示所有的数据可用但是部分副本还没有分配,红色表示部分数据因为某些原因不可用.

2. **获取集群的节点列表**
`curl 'localhost:9200/_cat/nodes?v'`

3. **列出所有索引**
`curl 'localhost:9200/_cat/indices?v'`

4. **创建索引**
`curl -XPUT 'localhost:9200/customer?pretty'`

5. **插入和获取**
现在我么插入一些数据到集群索引。我们必须给ES指定所以的类型。如下语句：
"external" type, ID：1:主体为JSON格式的语句： { "name": "John Doe" }
```shell
curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
{
       "name": "John Doe"
}'
```

##Documents##
[详细 API 解释 1](http://m.blog.csdn.net/pilihaotian/article/details/52452014)
[详细 API 解释 2](http://blog.csdn.net/tianyaleixiaowu/article/details/72844539)
[https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html)
[https://my.oschina.net/u/1246838/blog/353089](https://my.oschina.net/u/1246838/blog/353089)
[http://www.voidcn.com/blog/jingkyks/article/p-3255075.html](http://www.voidcn.com/blog/jingkyks/article/p-3255075.html)
[索引查询](https://www.baidu.com/link?url=KZf2T-Uan9BZYUA49hoEVkxmCHHkbLSzn_jiKGveXMTGJsJUjK4tlJCqjXOB_GioGW5cOfuJslpQ_mj96kvwaK&wd=&eqid=b08ea0220000797d00000005595dd4f7)
