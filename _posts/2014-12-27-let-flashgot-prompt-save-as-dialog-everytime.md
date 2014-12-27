---
layout: post
title: 让 Flashgot 每次都弹出保存对话框
date: 2014-12-27 00:50:00 +0800
description: ""
tags: [Firefox, Flashgot]
---

Flashgot 扩展应该可以算是 Firefox 的杀手锏扩展之一了。在我的网络环境中用 Flashgot + aria2c 可以满速下载百度云的视频（**10MB/S**），而同样的网络环境中用 Windows 7 下的百度云管家却只有 **3MB/S**。作为 Linux 玩家的我就算睡着了也能笑醒啊！

最近却有个 Flashgot 的问题一直困扰着我。第一次使用 Flashgot 的时候它会弹出一个保存对话框，让你选择下载文件的保存目录，可是自此以后 Flashgot 却再也不提示了，下载的文件总是保存在第一次选择的目录中，这让我很是郁闷。一番搜索之后终于在 [https://flashgot.net/faq](https://flashgot.net/faq) 找到了问题的原因和解决方法。

原来当 Flashgot 调用的外部程序（ aria2c ） 不能自己询问保存目录（ aria2c 是命令行程序 ）时，Flashgot 会去查询 Firefox 的首选项，以此来决定该把文件保存到哪里去。当 Firefox 的下载配置项没有设置为 *总是询问保存文件的位置* 时，就会出现上文描述的现象了。解决方法也很简单，在 Firefox 的首选项 -> 下载里面，选中 *总是询问保存文件的位置* 即可，这样 Flashgot 就会每次都弹出保存对话框啦。

**注**：  
*Firefox : 34.0.5*  
*Flashgot : 1.5.6.8* 
