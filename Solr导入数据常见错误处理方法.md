#Solr 6.6.0 data-import.xml Problem

1. `WARN SimplePropertiesWriter Unable to read: dataimport.properties`
需要在配置目录添加一个　`conf` 目录，目录下存放一个名为 `dataimport.properties` 的空文件
[http://egeek.io/simplepropertieswriter-unable-to-read-dataimport-properties/](http://egeek.io/simplepropertieswriter-unable-to-read-dataimport-properties/)
[https://cwiki.apache.org/confluence/display/solr/Uploading+Structured+Data+Store+Data+with+the+Data+Import+Handler](https://cwiki.apache.org/confluence/display/solr/Uploading+Structured+Data+Store+Data+with+the+Data+Import+Handler)

* * *

2. `JdbcDataSource
JdbcDataSource was not closed prior to finalize(),&#8203; indicates a bug -- POSSIBLE RESOURCE LEAK!!!`

* * *

3. `schema.xml: defaultSearchField has been deprecated and is incompatible with configs with luceneMatchVersion >= 6.6.0. Use 'df' on requests instead`
注视掉旧的默认搜索字段 `schema.cml`
```xml
<defaultSearchField>text</defaultSearchField>
<solrQueryParser defaultOperator="AND"/>
```
[https://stackoverflow.com/questions/40899348/after-upgrading-solr-4-10-to-6-3-the-search-stopped-working](https://stackoverflow.com/questions/40899348/after-upgrading-solr-4-10-to-6-3-the-search-stopped-working)

* * *

4. `Could not load driver: com.mysql.jdbc.Driver`
[下载链接](https://dev.mysql.com/downloads/connector/j/3.1.html) 下载后需要在 `solrconfig.xml` 中引入
[http://blog.csdn.net/programmeryu/article/details/77099863](http://blog.csdn.net/programmeryu/article/details/77099863)

* * *

5. `vorg.apache.solr.common.SolrException: [doc=112877] missing required field: content_type`

[Required Field must be marked as indexed or stored](https://stackoverflow.com/questions/9745039/solr-missing-required-field-error-when-the-field-is-not-missing)

6. `Solr 4 默认搜索字段`
[https://stackoverflow.com/questions/19263663/set-default-search-fields-in-apache-solr](https://stackoverflow.com/questions/19263663/set-default-search-fields-in-apache-solr)

7. `Solr AND OR 查询问题`
or和and不能用小写的；要用大写的OR, AND。
[https://segmentfault.com/q/1010000007900916](https://segmentfault.com/q/1010000007900916)

##Documents
[Solr实现SQL的查询与统计](http://www.aboutyun.com/thread-7742-1-1.html)
[Solr 多字段、打分规则、权重和实时索引同步](http://www.cnblogs.com/netuml/p/5695751.html)
[Solr 删除所有索引](http://blog.csdn.net/qing419925094/article/details/42142117)