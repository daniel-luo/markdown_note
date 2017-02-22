##Install Grunt##
1. 安装 Nodejs
2. Nodejs 安装好后, 安装 Npm
3. 到项目根目录下去，确保存在 `Gruntfile.js` 和 `package.json` 这两个文件
4. npm install, 他会自动安装需要的依赖包
5. npm instal grunt


要用 Chrome 看 map 文件
1. grunt refresh
2. grunt less

这里需要配置主题
`/public_html/dev/tools/grunt/configs/themes.js`
```js
    hollywoodsuits: {
        area: 'frontend',
        name: 'Silk/hollywoodsuits',
        locale: 'en_US',
        files: [
            'css/styles-m',
            'css/styles-l'
        ],
        dsl: 'less'
    }
```

###相关资料###
[http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/css-topics/css_debug.html](http://devdocs.magento.com/guides/v2.0/frontend-dev-guide/css-topics/css_debug.html)