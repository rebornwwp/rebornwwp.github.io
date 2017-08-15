---
layout: post
title:  "国内网页应用开发CDN链接被墙的应对方法"
date:   2016-10-07 23:23:02 +0800
comments: true
tags:
- programming
- '中文'
---
自从lantern被墙,换了新的vpn以来，从google cdn, bootstrap cdn, cloudflare cdn加载资源速度变得超慢，请求CDN资源能花上半分钟，超级令人不爽(`‐●_‐´怒)。


解决方法有：

1. 把资源下载下来放在专门存放静态文件的文件夹中
2. 如果网站主要面向的是国内的用户，换成国内的CDN也无妨。
3. vpn开成全局模式。


如果国内的CDN的话可以用：

-  http://www.bootcdn.cn/ 
流行的js库和css框架基本上都有

- 新浪和微软的CDN资源也很全，网上搜一搜就会有具体的资源地址




如果还是速度很慢的话，
一定是你家的宽带有问题啊ああああああああ (´￢O￢)
