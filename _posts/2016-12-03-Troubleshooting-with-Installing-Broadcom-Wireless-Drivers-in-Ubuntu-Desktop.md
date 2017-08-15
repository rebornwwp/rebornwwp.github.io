---
layout: post
title:  "Troubleshooting with Installing Broadcom Wireless Drivers in Ubuntu Desktop"
date:   2016-12-02 20:23:06 +0800
comments: true
tags:
- programming
---

Days before, I changed the operating system of my old laptop from Windows7 to Ubuntu Desktop, I installed Ubuntu from a USB stick from my friend. After installing I found I Broadcom wireless driver wasn't installed so I couldn't connect to my WiFi. I tried several ways then found the solution, hope it my be helpful.

To download the driver from Internet, you need to connect with Ethernet.


Open Software and Update from system and configuration, open the fifth tab optional drivers and see if wireless driver comes out or not.


<img src="/img/ubuntu1.png" style="height:50%;width:50%;">

Make sure allowing downloading drivers from Internet is checked in the first tab Ubuntu Software.


<img src="/img/ubuntu2.png" style="height:50%;width:50%;">


If it still doesn't come out, you need to manually install it from terminal, use `lspci -nn -d 14e4:` command to see which network card you're using, then install the corresponding driver. <a href="http://askubuntu.com/questions/55868/installing-broadcom-wireless-drivers">http://askubuntu.com/questions/55868/installing-broadcom-wireless-drivers</a> offers an excellent guide.
