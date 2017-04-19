##Python 数据抓取##

```python
from pyquery import PyQuery as pyq
import sys  
reload(sys)  
sys.setdefaultencoding('utf8')  

doc=pyq(url=r'https://detail.1688.com/offer/546755205911.html')
# cts=doc('.mod-detail-title')
 
# for i in cts:
#     print '====',pyq(i).find('h1').text() ,'===='


title = doc('h1.d-title').text()
print title
price = doc('#mod-detail-price').text()
print price
objLeading = doc('.obj-leading').text()
print objLeading
objSku = doc('.obj-sku').text()
print objSku
company = doc('.company-name').find('a.name').text()
print company
factory = doc('.supplierinfo-common').find('.detail').find('.biz-type').find('label').text()
fmodel  = doc('.supplierinfo-common').find('.detail').find('.biz-type-model').text()
print factory , fmodel
region = doc('.supplierinfo-common').find('.detail').find('.address').find('label').text()
address = doc('.supplierinfo-common').find('.detail').find('.address').find('.disc').text()
print region, address
detail = doc('#mod-detail-attributes').find('table').text()
print detail
detailDesc = doc('#mod-detail-description').text()
print detailDesc
```

##Documents##
[Python写爬虫——抓取网页并解析HTML](http://www.cnblogs.com/bluestorm/archive/2011/06/20/2298174.html)
[python模块学习---HTMLParser(解析HTML文档元素)](http://www.cnblogs.com/myyan/p/4846391.html)
[python 编码问题：'ascii' codec can't encode characters in position 的解决方案 ](http://blog.csdn.net/olanlanxiari/article/details/48201231)