##Mysql把一张表的数据写入到另一张表
**1. 表结构完全一样**
```sql
 insert into 表1
 select * from 表2
```

**2. 表结构不一样（这种情况下得指定列名)**
```sql
insert into 表1 (列名1,列名2,列名3)
select  列1,列2,列3 from 表2
```

**3、只从另外一个表取部分值**
```sql
insert into 表1 (列名1,列名2,列名3) values(列1,列2,(select 列3 from 表2));
```

###Question
使用第三种方法，当查询的结果不止一条数据时会的到错误:`subquery returns more than 1 row`
正确使用方式:
```sql
INSERT INTO `solr_reindex_queue` (
	`product_id`,
	`type_id`,
	`status`,
	`action`,
	`created`
)
SELECT
parent_id AS product_id, 'configurable' AS type_id, 'pending' AS status, 'A' AS action, NOW() AS created
FROM
`catalog_product_super_link`
GROUP BY
parent_id
```
子查询的字段和`insert`保持一致，不要用`INSERT`关键字`VALUES`