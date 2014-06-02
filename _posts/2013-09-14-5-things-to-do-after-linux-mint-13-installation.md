---
layout: post
title: 安装linux mint 13 之后五件应该做的事情
description: ""
tags: [linux, mint 13,]
---


##### 打开网络连接
安装完linux mint 13之后，我第一件想做的事情就是联网安装“装机必备”软件（比如 google chrome 和 smplayer）。可令我感到沮丧的是，在系统设置 -> 网络里面，我竟然无法添加 DSL/PPPoE 拨号链接！费了老大劲才在 *preferences-> 网络连接* 里找到。要打开网络连接，有三种途径：
-   点击左下角菜单 -> preferences -> 网络连接
-   按windows键 -> 直接键入 connect -> 点击回车键就可打开网络连接
-   直接在终端中输入命令 `nm-connection-editor`

#### 修改软件源
修改软件源为ubuntu网易镜像(具体使用那个ubuntu国内源得看你的实际情况)。由于linux mint的软件源里自带的中国镜像是教育网的，而我的拨号宽带是中国电信，这促使我萌生了换软件源的想法。linux mint安装软件时很多软件包都是直接从ubuntu的仓库获取的，既然这样，那我把apt配置文件中的ubuntu仓库替换成ubuntu网易镜像即可。
-  在终端中运行下述命令，打开apt软件源配置文件：
{% highlight bash %} 
cd /etc/apt/
sudo cp sources.list sources.list.bk.after.installed
gksudo gedit /etc/apt/sources.list
{% endhighlight  %} 

-  使用下面的配置把sources.list文件的原有内容替换掉即可，高亮部分是我改动的内容。特别提醒下，建议在替换之前，还是仔细看下原有内容和我的配置的差别，主要是怕你的sources.list文件已经添加了一些其它的软件源。

{% highlight bash %} 
deb http://packages.linuxmint.com/ maya main upstream import

# 原文件内容， mint 直接使用了ubuntu的软件源
# deb http://archive.ubuntu.com/ubuntu/ precise main restricted universe multiverse
# deb http://archive.ubuntu.com/ubuntu/ precise-updates main restricted universe multiverse
# deb http://security.ubuntu.com/ubuntu/ precise-security main restricted universe multiverse

# 修改后内容，mint使用了 163 镜像的软件源
deb http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse

deb http://archive.canonical.com/ubuntu/ precise partner
deb http://packages.medibuntu.org/ precise free non-free

# deb http://archive.getdeb.net/ubuntu precise-getdeb apps
# deb http://archive.getdeb.net/ubuntu precise-getdeb games
{% endhighlight  %} 

##### 安装中文语言支持
由于我安装linux mint的过程中是没有网络连接可用的，所以安装完成后，中文语言支持并不完整，需要在联网之后重新检查并安装语言支持。点击左下角菜单 -> system tools -> 系统设置 -> 语言支持，按照提示操作即可完成。

##### 安装nvidia的私有驱动
点击左下角菜单 -> preferences -> 附加驱动 -> 选择 NVIDIA 图形加速驱动(版本 current) [推荐] -> 点击激活按钮。

##### 安装ibus， ibus-googlepinyin 和 ibus-sunpinyin 输入法

{% highlight bash %} 
sudo apt-get install ibus ibus-googlepinyin ibus-sunpinyin
{% endhighlight  %} 
