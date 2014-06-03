---
layout: post
title: 在ubuntu 12.04 中安装SciTE 文本编辑器
description: ""
tags: [ubuntu, SciTE]
---


##### SciTE 介绍
 
SciTE 是一款基于 SCIntilla 开发的文本编辑器。最开始 SciTE 是为了演示 Scintilla 而开发，但是逐渐发展成了一款非常有用的用于构建、运行程序的程序员编辑器，支持众多的脚本，比如C、C++、PHP、C\#、perl、html、css、java 等等。免费而且开源。支持 linux 和 win32 环境。在 windows 下面只需要将下载的文件解压缩就能使用。较其它同重量级软件，最耀眼的就是导出功能，可以导出PDF/HTML/RTF/XML/LaTex类型的文件，直接就能将语法高亮的内容导出。

##### 安装

{% highlight bash linenos %}
sudo apt-get install scite
wget http://scite-files.googlecode.com/svn-history/trunk/translations/locale.zh_cn.properties
mv locale.zh_cn.properties locale.properties
sudo mv locale.properties  /usr/share/scite
{% endhighlight %}

##### 软件截图

![scite snapshot](http://i.imgur.com/g6HfK.png)
