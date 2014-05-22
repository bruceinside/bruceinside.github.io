---
layout: post
title: 调教 kaffeine 正确显示中文字幕
date: 2014-05-23 01:00:00 +0800
description: "详细分析了 kaffeine 的字幕处理 （ 其实本质上是 XINE 的问题） 存在的缺陷，以及如何让 kaffeine 正确显示 GBK 编码的中文 srt 字幕。"
tags: [linux, openSUSE, kaffeine, xine ]
---

我们接着上篇文章《[调教 kaffeine 播放各类媒体格式]({% post_url 2014-05-21-setup-kaffeine-to-play-various-media-formats %})》，继续讲讲 kaffeine 的字幕问题。
[kaffeine](http://www.kde.org/applications/multimedia/kaffeine/) 的字幕处理能力不甚理想。其实我不想一开始就批评 kaffeine 这不好那不好， 这会让我的这篇博文变成批判大会，让我的博文偏离主题，可是我怎么就忍不住呢？ kaffeine 的字幕处理 （ 其实本质上是 XINE 的问题） 存在如下缺陷：

0. 只支持 srt 格式的字幕，不支持 ass / ssa 格式的。
0. 字幕不能使用系统字体，只能使用 XINE 自己的原生字体或者 TTF 字体。对于从 packman 中安装的 `libxine2` 包来说，自带的原生字体（ 在 `/usr/share/xine-lib/fonts/` 目录下 ）都不能正确显示中文，需要额外安装中文字体。
0. 只会自动加载 `video-name.srt` 这样命名的字体，对于 `video-name.chs.srt` 这样命名的则不能自动加载。
0. 可以手动加载字幕，可是却没有直观的菜单供用户去选择字幕文件。用户需要在播放视频的时候 *点击左边的播放列表页签 -> 右键点击右上角播放列表中的视频文件标题 -> 添加字幕* 来进行手动加载字幕，用户不仔细研究是根本想不到藏在这里的。
0. 不能自动识别外置字幕文件的编码。默认情况下编码为 `iso-8859-1`。 对于中文用户来说，绝大部分的字幕文件都是 GBK 编码的，这都会显示为乱码。
0. 字幕的背景为绿色，这应该是 XINE 的一个 bug，看起来我们也无能为力。

由于上述的诸多问题，我们的问题域就大大缩小了，被迫变成：*怎样才能正确显示 GBK 编码的中文 srt 字幕?* 下面我以 openSUSE 13.1 系统为例进行讲解。

##### 安装 XINE 的中文原生字体

[hillwood 的 OBS 个人仓库](http://download.opensuse.org/repositories/home:/hillwood/)中提供了两个 XINE 的中文原生字体包，分别是 `xine-font-wqymh` （文泉驿微米黑） 和 `xine-font-wqyzenhei` （文泉驿正黑），有点小遗憾的是只提供了 openSUSE 12.2 和 openSUSE 12.3 的，且只能用于 libxine1， 不过其实没有什么大的妨碍，因为字体文件其实与发行版本无关，我们大可以把需要的字体文件从 RPM 包中抽取出来，直接放到 XINE 的字体目录中。XINE 的字体在 `libxine2` 包中，路径是 `/usr/share/xine-lib/fonts/` ， 你可以通过 `rpm -ql libxine2` 看出来。我以`xine-font-wqymh` 为例，先下载 [xine-font-wqymh-0.9.41-3.1.noarch.rpm](http://download.opensuse.org/repositories/home:/hillwood/openSUSE_12.3/noarch/xine-font-wqymh-0.9.41-3.1.noarch.rpm) 包，然后把里面所有的 `xinefont.gz` 文件解压到 `/usr/share/xine-lib/fonts/` 下， 解压后的 `/usr/share/xine-lib/fonts/` 看起来应该是这样的：

{% highlight bash %}
/usr/share/xine-lib/fonts/
# 省略了其它内置字体
├── wqymicrohei-16.xinefont.gz
├── wqymicrohei-20.xinefont.gz
├── wqymicrohei-24.xinefont.gz
├── wqymicrohei-32.xinefont.gz
├── wqymicrohei-48.xinefont.gz
├── wqymicrohei-64.xinefont.gz

{% endhighlight %}

这样之后，要让 kaffeine 使用上我们安装的字体，还得修改下 kaffeine 的这个配置文件 `~/.kde4/share/apps/kaffeine/xine-config`，我的配置项如下：

{% highlight bash %}
# font for subtitles
# string, default: sans
subtitles.separate.font:wqymicrohei
{% endhighlight %}
字体名称就是那些 `xinefont.gz` 文件第一个连字符前面的部分。

##### 修改 XINE 字幕编码

这个就相对容易多了，同样是修改 `~/.kde4/share/apps/kaffeine/xine-config`，我的配置项如下：

{% highlight bash %}
# encoding of the subtitles
# string, default: iso-8859-1
subtitles.separate.src_encoding:GBK
{% endhighlight %}

反正是你的字幕文件用什么编码，你就把这个配置项修改为对应编码的名称即可。最常见的中文字幕编码就两种： GBK 和 UTF-8。

最后附上我的 kaffeine 截图，一飨读者：      
![kaffeine-with-gbk-chinese-subtitle-correctly-displayed.png](http://suselinks-us.qiniudn.com/kaffeine-with-gbk-chinese-subtitle-correctly-displayed.png)

注：      
kaffeine 版本： 1.2.2      
libxine2 包版本: 1.2.5， 从 packman 安装的      


