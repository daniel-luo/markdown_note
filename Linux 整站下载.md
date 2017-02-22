##Linux 整站下载##

**方法一**

```shell
wget -m -e robots=off -U "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6" "http://local.xhprof.com/"
```

**方法二**

```shell
wget -r -np -k "http://local.xhprof.com/"
```

下载的时候，尽可能缩小你的下载范围。

下面给出参数的说明：
-r, --recursive（递归下载） specify recursive download.
-k, --convert-links（转换链接为本地链接） make links in downloaded HTML point to local files.
-p, --page-requisites（下载所有的图片等页面必需元素） get all images, etc. needed to display HTML page.
-np, --no-parent（不追溯至父级） don't ascend to the parent directory. 