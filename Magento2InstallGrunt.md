##Magento2 install Grunt##
1. 修改 package.json 中`"grunt-xxxxx": "latest",`的版本号为 `lastest`
2. 在 Magento2 根目录执行 `npm install`
~~3. `npm dedupe --save-dev` 否则 `watch` 时会出现 `ENOSPC` 错误导致无法监听~~
3. 如果出现 ENOSPC 错误，执行一下语句,详见**错误ENOSPC**

###livereload###
[http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/css-topics/css_debug.html](http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/css-topics/css_debug.html)
[https://github.com/guard/guard-livereload](https://github.com/guard/guard-livereload)
[http://livereload.com/extensions/](http://livereload.com/extensions/)
[https://github.com/magento/magento2/issues/1619](https://github.com/magento/magento2/issues/1619)为什么watch监听不起作用

##Setup##
1. `grunt deploy` 部署静态文件
2. `grunt refresh`
3. `grunt clean`
4. `grunt exec:youThemeName`
5. `grunt less:youThemeName`
6. `grunt watch`

grunt refresh
grunt less
grunt watch
deploy

##LiveReload extensions##
Firefox[https://addons.mozilla.org/en-US/firefox/addon/livereload/](https://addons.mozilla.org/en-US/firefox/addon/livereload/)
Document[http://livereload.com/](http://livereload.com/)
[http://livereload.com/extensions/](http://livereload.com/extensions/)

[http://blog.csdn.net/coffeesweet/article/details/50926847](http://blog.csdn.net/coffeesweet/article/details/50926847)


1. Remove Cache
2. Clean theme by command : grunt clean
3. Run CMD command prompt with administrator privilege.
4. run command : grunt exec:yourthemename
5. run command : grunt less:yourthemename
6. run command : grunt watch


1. grunt clean
2. grunt exec:themeName
3. grunt less:themName
4. grunt watch

##错误 ENOSPC##
```shell
echo fs.inotify.max_user_watches=524288 | sudo tee –a /etc/sysctl.conf && sudo sysctl –p
```

`npm dedupe`
[http://stackoverflow.com/questions/16748737/grunt-watch-error-waiting-fatal-error-watch-enospc](http://stackoverflow.com/questions/16748737/grunt-watch-error-waiting-fatal-error-watch-enospc)