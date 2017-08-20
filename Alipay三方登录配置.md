##Alipay三方登录配置##

##Documents##
[配置平台页面](https://open.alipay.com/platform/home.htm)
[https://doc.open.alipay.com/docs/doc.htm?spm=a219a.7386797.0.0.PInaVk&treeId=263&articleId=105809&docType=1](https://doc.open.alipay.com/docs/doc.htm?spm=a219a.7386797.0.0.PInaVk&treeId=263&articleId=105809&docType=1)
[获取用户信息](https://doc.open.alipay.com/docs/doc.htm?spm=a219a.7629140.0.0.fg8JZJ&treeId=193&articleId=106001&docType=1)
[三方登录沙箱配置](http://www.cnblogs.com/AndroidJotting/p/6515268.html)
[http://www.cnblogs.com/AndroidJotting/p/6515268.html](http://www.cnblogs.com/AndroidJotting/p/6515268.html)
[网站支支付宝登录SDK](https://doc.open.alipay.com/docs/doc.htm?spm=a219a.7629140.0.0.igoqoM&treeId=263&articleId=106747&docType=1)
[使用OpenSSL工具生成密钥](https://doc.open.alipay.com/docs/doc.htm?articleId=106130&docType=1)
[windows生成密钥](https://doc.open.alipay.com/docs/doc.htm?treeId=291&articleId=105971&docType=1)
[自定义授权](http://www.moke8.com/article-10287-1.html)
[APP配置](https://doc.open.alipay.com/docs/doc.htm?spm=a219a.7629140.0.0.ryV3KF&treeId=291&articleId=105972&docType=1)
```shell
OpenSSL> genrsa -out app_private_key.pem   1024  #生成私钥
OpenSSL> pkcs8 -topk8 -inform PEM -in app_private_key.pem -outform PEM -nocrypt -out app_private_key_pkcs8.pem #Java开发者需要将私钥转换成PKCS8格式
OpenSSL> rsa -in app_private_key.pem -pubout -out app_public_key.pem #生成公钥
OpenSSL> exit #退出OpenSSL程序
```

```shell
ID: 15
Key: ziLjNI8ScZ64
Secret: 2BMEJopbl3Je3B30WmoHGZDsTOGvZcVYZXvKhqbSpsngPedW
```