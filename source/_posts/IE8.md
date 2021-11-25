---
title: IE8相关
date: 2018-05-02 11:16:33
tags:
---
IE8兼容性相关。
![](/images/ie8.jpg)

### 1.background-size: length|percentage|cover|contain
IE9+、Firefox 4+、Opera、Chrome 以及 Safari 5+ 支持 background-size 属性。
由于 background-size 是 CSS3 新增的属性，所以 IE 低版本不支持。
兼容方法：
``` bash
.box {
	width: 30px;
	height: 30px;
	background: url(../img/picture.png) no-repeat left top;
	background-size: contain;
	-ms-filter: "progid:DXImageTransform.Microsoft.AlphaImageLoader(src='img/picture.png',sizingMethod='scale')";
	filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(src='img/picture.png',sizingMethod='scale');
	background: none\9;
    }
```

### 2.border-radius 圆角
圆角在ie8下也是不支持的，需要下载一个文件PIE.htc，代码如下：
``` bash
border-radius: 25px;
/*兼容圆角*/
-ms-behavior:url(/portal/themes/boss/login/css/PIE.htc);
behavior: url(/portal/themes/boss/login/css/PIE.htc);
```
