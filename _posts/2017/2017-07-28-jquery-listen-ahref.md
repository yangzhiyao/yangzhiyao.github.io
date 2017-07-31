---
title: 拦截当前页面外部链接
---

前因：采取技术措施,做到在用户点击链接离开党政机关网站时予以明确提示。

后果：需要把网站改一下

思路：怎么改能实现这个目的，当然我说的是在尽可能少费功夫的前提下

1、修改模板页面，用php过滤所有输出的url，使用重定向实现
2、添加一个全局js，获取当前页面所有链接，判断不是本网站的链接，弹出警告框

针对客户目前的情况，还是后者简单些不需要改服务端应用，那么就准备开始吧！

```js
$(function(){

// 得到当前域名
var host = window.location.host;

// 得到当前页面所有链接
var links = $("a");

// 判断是否是站内链接，如若不然，弹出提醒：“您点击的链接是站外链接，是否确认？”
links.click(function(event){
    if(isOuter($(this).href)){
        var message = "您点击的链接是站外链接，是否确认继续访问？";
        var isGo = confirm(message);
        if(!isGo)
            event.preventDefault();
        }
    } 
})

// isOuter定义
function isOuter(url){
    if(url.match(/^http[s]?:\/\//)){
        if(url.indexOf(host) != -1){
            return true;
        }
    }
    return false;
}

})
```