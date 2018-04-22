---
layout: post
title:  "如何在ubuntu16通过终端设置shadowsocks实现科学上网"
date:   2018-04-22 17:47:06 +0800
comments: true
tags:
- programming
---

给三年前买的旧笔记本重装了Ubuntu桌面版，第一件要做的事就是科学上网啦。

shadowsocks的安装有两种方法，一是安装shadowsocks-qt5 GUI版，二是通过终端运行shadowsocks。我试过了第一种GUI安装，但是一直连不上服务器，发现输出的json配置文件格式是错误的，于是选择了第二种, 具体步骤如下:

<hr>


### 1. 下载shadowsocks

```
sudo apt-get update
sudo apt-get install python-pip
sudo apt-get install python-setuptools m2crypto
pip install shadowsocks
```

<br/>

### 2. 设置配置文件

```
{
"server":"XX.XX.XX.XX",
"server_port":XX,
"local_port":1080,
"password":"your password",
"timeout":600,
"method":"aes-256-cfb"
}
```

<br/>

### 3. 启动shadowsocks

```
sslocal -c /your/path/shadowsocks.json
```

如果启动成功了可以看到如下信息(/home/momoko/shadowsocks/ss-config.json是我存放配置文件的路径)

```
INFO: loading config from /home/momoko/shadowsocks/ss-config.json
2016-12-06 19:31:54 INFO     loading libcrypto from libcrypto.so.1.0.0
2016-12-06 19:31:54 INFO     starting local at 127.0.0.1:1080
```

<br/>

### 4. 设置浏览器

接下来需要设置浏览器，代理到shadowsocks指定端口(默认情况是1080)

我使用的浏览器是Ubuntu下的Chromium，chrome插件本来可以在chrome商店里下载到，成功翻墙之前是进不去的。这里推荐从github上下载SwitchyOmega，里面有中文下载说明, 网址如下:

<a href="https://github.com/FelisCatus/SwitchyOmega/releases/">https://github.com/FelisCatus/SwitchyOmega/releases/</a>

还有配合GFWList的使用方法:

<a href="https://github.com/FelisCatus/SwitchyOmega/wiki/GFWList">https://github.com/FelisCatus/SwitchyOmega/wiki/GFWList</a>

下载结束之后需要设置代理选项，情景模式里选`socks 5`, 端口号`1080`，其余设置参考上述链接。

打开youtube或是facebook测试一下，有点小惊喜呢。┌|≧∇≦|┘

<br/>

