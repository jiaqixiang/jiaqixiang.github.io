---
title: RSA加密
date: 2019-05-03 19:04:41
tags:
---

vue相关的rsa加密,项目需求,留作回顾.

![](/images/rsa.jpg)

### vue组件引入
``` bash
import { JSEncrypt } from 'jsencrypt';
```

### 方法内使用
``` bash
let publicKey = asdfsafdadfafasjdhfasfd // 从后台获取公钥，这里省略，直接赋值

let encryptor = new JSEncrypt()  // 新建JSEncrypt对象
encryptor.setPublicKey(publicKey)  // 设置公钥
let rsaPassWord = encryptor.encrypt(password)  // 对密码进行加密
```
