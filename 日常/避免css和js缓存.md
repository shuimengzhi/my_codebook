```
<link rel="stylesheet" type="text/css" href="css/page.css?version=1001"/>
<script src="jquery.min.js?version=1001"></script>
```
不用改原始的css和js文件名，只需在引入页面加上一个参数，一般为js、css版本号。

如果项目更新了css或者js文件，可以在文件中统一替换版本号，这样客户端会重新加载css和js文件，避免了缓存。