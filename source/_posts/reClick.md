---
title: 重复点击
date: 2019-03-01 18:51:11
tags:
---

防止重复点击,或者点击过快方法.

![](/images/reClick.jpg)

### 1.html
``` bash
<button type="button" data-num="0">00000</button>
<button type="button" data-num="1">11111</button>
<button type="button" data-num="2">22222</button>
```

### 2.js
``` bash
<script>
var isClick = true;
$("button").on("click",function(){
    if(isClick) {
        isClick = false;
        //事件
        console.log($(this).attr("data-num"));
        //定时器
        setTimeout(function() {
            isClick = true;
        }, 1000);//一秒内不能重复点击
    }
});
</script>
```

### 实际需求代码
![](/images/reCli.jpg)
