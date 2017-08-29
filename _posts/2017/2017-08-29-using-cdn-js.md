---
title: 使用cdn加载js库
---

## 常用的库地址列表
https://developers.google.com/speed/libraries/
https://cdnjs.com/
http://www.bootcdn.cn/
http://cdn.code.baidu.com/
https://www.staticfile.org/
http://jscdn.upai.com/
http://lib.sinaapp.com/
https://docs.microsoft.com/en-us/aspnet/ajax/cdn/overview

## 用法
```js
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script>!window.jQuery && document.write('<script src="/js/jquery.min.js"></script>');</script>
```

当然，google在官G方F网W的影响下可能不好使，所以建议在 CDN 读取失败的时候从自己服务器提供。