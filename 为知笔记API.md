####通过个人mywiz邮箱把邮件转发到指定笔记目录或添加指定标签

发邮件时，在邮件标题中增加 #标签名# 即可，多个标签用逗号分割。

例1：
```
邮件标题#标签1#
```

例2：
```
邮件标题#标签1，标签2,标签3#
```

发邮件时，在邮件正文增加'folder=目录名称'，
** 例如： **
```
folder=/读书笔记/三国演义/
```
就可以把邮件保存到个人笔记的指定目录下。

######为知API
[http://www.wiz.cn/faq/common/api](http://www.wiz.cn/faq/common/api)

** 插件开发帮助文档 **

Windows客户端插件开发手册： [http://www.wiz.cn/manual/plugin/](http://www.wiz.cn/manual/plugin/)

** 内容收集 **

通过写程序发邮件保存内容到为知笔记：
1. 通过发邮件到 ** [mywiz](http://www.wiz.cn/wiz-mywiz.html) **邮箱实现内容保存到为知笔记。
2. Url2Wiz：写程序发送 HTTP GET请求，将指定URL对应的网页内容保存到为知笔记。

http://www.wiz.cn/manual/plugin/