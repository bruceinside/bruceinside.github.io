---
layout: post
title: 调教 kaffeine 播放各类媒体格式
date: 2014-05-21 23:30:00 +0800
description: "详细介绍了如何让 kaffeine 播放 rmvb, mkv, mp4, wmv, m4v, mov, 3gp, 3g2 等视频文件。"
tags: [linux, openSUSE, kaffeine, xine ]
---

其实我一直对 openSUSE 把 [kaffeine](http://www.kde.org/applications/multimedia/kaffeine/) 作为默认视频播放器感到不解。论功能，[VLC](http://www.videolan.org/) 不知道要强多少倍，[获得的奖项](https://wiki.videolan.org/VLC_Awards)多不胜数； 论简单, 这是 [Dragon Player](http://www.kde.org/applications/multimedia/dragonplayer/) 的专注方向，而且这两者都是免费开源的，符合 openSUSE 收录软件的政策。可为什么要把 kaffeine 这款基于 [XINE](http://www.xine-project.org/home) 的前端作为默认视频播放器呢？当前 linux 世界中视频播放器主要有 3 种 （XINE, Mplayer, VLC） ，我不提功能，单从使用率和曝光率来说，XINE 怕是要倒数第一啊！

不过话说回来，XINE 的功能并不弱，至少我在拖动视频播放进度条时，发现响应速度还是比 VLC 快的。

使用 openSUSE 的用户在用 kaffeine 打开视频文件时，会发现 kaffeine 总是提示如下信息：

![kaffeine-prompt-to-install-extra-codecs.png](http://suselinks-us.qiniudn.com/kaffeine-prompt-to-install-extra-codecs.png)

这其实不能怪 kaffeine，kaffeine 是基于 `libxine2` （ 其实就是 XINE 官方所说的 `xine-lib` ） 包的， openSUSE 的政策是[不收录受专利和版权保护的软件](http://en.opensuse.org/Restricted_formats)，所以 openSUSE 官方提供的 `libxine2` 缺少一些插件，因而像 rmvb, mkv, mp4, wmv, m4v, mov, 3gp, 3g2 等视频文件的编解码器，openSUSE 系统是不能默认安装的。但是，用户自己是可以的，毕竟，要对公司发起诉讼容易，但是对世界上的海量用户一一发起诉讼，那是比蜀道还难呀！

那用户要怎么安装这些编解码器呢？ 我们先说安装 mkv, mp4, wmv, m4v, mov, 3gp, 3g2 等视频文件的编解码器，这些可以很方便的从 [packman](http://packman.links2linux.org/) 安装。核心的包是 `libxine2-codecs`，我以 openSUSE 13.1 为例进行讲解：

{% highlight bash %}
sudo zypper ar -f http://packman.inode.at/suse/openSUSE_13.1/ packman
sudo zypper update
sudo zypper install libxine2-codecs
{% endhighlight %}

这样之后，mkv, mp4, wmv, m4v, mov, 3gp, 3g2 等视频文件都可以播放了，但是唯独 rmvb 文件不行。这个的安装稍显复杂。对于 32 位系统，你需要安装 `w32codec-all` 包，该包同样由 packman 提供。

{% highlight bash %}
sudo zypper install w32codec-all
{% endhighlight %}

安装完成之后呢，编辑下 `~/.kde4/share/apps/kaffeine/xine-config` 文件， 把 `decoder.external.real_codecs_path` 配置的值修改为 `/usr/lib/win32`。 下面是示例配置:

{% highlight bash %}
# path to RealPlayer codecs
# string, default: 
decoder.external.real_codecs_path:/usr/lib/win32
{% endhighlight %}

修改的时候有两点需要注意：

0. 修改 xine-config 文件前务必确保 kaffeine 已经关闭； kaffeine 程序在退出的时候会把当前配置全部写入 xine-config 文件，先退出是为了防止你做的修改被丢弃。
0. 冒号的前后不能有空格啊！

这样你再启动 kaffeine 就可以播放 rmvb 文件啦～

对于 64 位系统来说，步骤稍微复杂点，这是因为 packman 没有提供 `w64codec-all` 这个包导致的（怨念），需要我们手动从 mplayer 的官方站点下载。要下载的包是 [essential-amd64-20071007.tar.bz2](http://www.mplayerhq.hu/MPlayer/releases/codecs/essential-amd64-20071007.tar.bz2)，下载完成之后，在 `/usr/lib64` 目录下建立 `win64` 目录，然后把 `essential-amd64-20071007.tar.bz2` 的内容解压到 `/usr/lib64/win64` 下。`/usr/lib64/win64` 目录结构如下：

{% highlight bash %}
/usr/lib64/win64
├── cook.so
├── drvc.so
├── README
└── sipr.so
{% endhighlight %}

然后就是修改 `~/.kde4/share/apps/kaffeine/xine-config` 文件：

{% highlight bash %}
# path to RealPlayer codecs
# string, default: 
decoder.external.real_codecs_path:/usr/lib64/win64
{% endhighlight %}


至此为止，所有的视频格式都可以正常播放了。我用 [http://support.apple.com/kb/HT1425](http://support.apple.com/kb/HT1425) 页面提供的样本视频文件以及自己电脑上 RV40 编码的 rmvb 文件测试了一遍，全部正常!

顺带提一句， rmvb 文件的解码是由 `drvc.so` 文件提供的。而且尽管我这篇文章是用 openSUSE 为例进行讲解，但是实际上整体思路是和具体的 linux 发行版本无关的，是适用于所有 linux 发行版本的。
