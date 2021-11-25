---
title: Date日期相关
date: 2018-08-30 18:39:18
tags:
---

获取当前时间日期及大小比较。

![](/images/timg.jpg)

### 获取当前时间，格式YYYY-MM-DD
``` bash
function getNowFormatDate() {
    var date = new Date();
    var seperator1 = "-";
    var year = date.getFullYear();
    var month = date.getMonth() + 1;
    var strDate = date.getDate();
    if (month >= 1 && month <= 9) {
        month = "0" + month;
    }
    if (strDate >= 0 && strDate <= 9) {
        strDate = "0" + strDate;
    }
    var currentdate = year + seperator1 + month + seperator1 + strDate;
    return currentdate;
}
```

### 比较日期大小
``` bash
function compareDate(date1, date2) {
    var date1 = new Date(date1);
    var date2 = new Date(date2);
    if (date1.getTime() > date2.getTime()) {
        return true;
    } else {
        return false;
    }
}
```