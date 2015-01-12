---
layout: post
title: openSUSE 13.2 系统中播放视频时开启 AMD 显卡硬件加速视频解码的方法
date: 2015-01-12 20:30:00 +0800
description: "该文详细讲解了openSUSE 13.2 系统中播放视频时开启 AMD 显卡硬件加速视频解码的方法"
tags: [openSUSE, VAAPI, AMD/ATI, Video Decoding, Hardware Acceleration, XvBA]
---

首先得声明一下，本文探讨的是 AMD（含ATI）显卡在 linux 操作系统下的视频播放硬件加速解码的方法，不涉及 Nvidia 显卡，也不涉及 Windows 等任何非 linux 操作系统，尽管该方法背后的思路对于它们来说是有参考价值的。

##### 视频播放硬件加速解码方案

先来看看本文采用的视频播放硬件加速解码方案的整体结构图：
![视频播放硬件加速解码整体结构图](http://suselinks-us.qiniudn.com/video-decoding-hw-accel-architecture.png)

现在我要按照从底至上的顺序介绍上图中的各个组成部分。

0. 由于该方案采用的是 XvBA 视频加速（最中间的VAAPI基于XvBA的实现），所以对底层的AMD显卡是有要求的，当前仅仅可用于那些支持 UVD2 的 AMD Radeon HD 显卡，主要是 Radeon HD 4000 系列及以后的显卡。

0. 同样由于采用 XvBA 的原因，也限制了该方案只能采用 AMD 提供的私有驱动，社区提供的开源驱动是不受支持的。

0. 现在说到最中间的基于 XvBA 的 VAAPI 实现了。XvBA 是 X-Video Bitstream Acceleration 的首字母缩写，它实现了 AMD 的 UVD2 引擎， 可以提高 linux 桌面系统中的视频解码和回放处理能力。它使得支持 VAAPI 的视频解码程序可以使用 VAAPI 接口，继而使用 XvBA 来调用 AMD 的 UVD2 视频解码和回放。有点遗憾的是当前的实现只支持对那些以 MPEG-4 AVC (H.264) and WMV9 (VC-1) 编码的视频文件进行加速，和 Nvidia 私有驱动中的 VDPAU 实现比较起来有更多的限制。所以下载高清视频文件时尽量选择那些后缀为 MKV 和 MP4 的文件，这些文件一般都是 H264 编码的。XvBA 其实是和 VAAPI、VDPAU 同一层面上的概念，也是 VAAPI、VDPAU 的直接竞争对手，只是在我这个方案中被用来做 VAPPI 的后台实现了，主要还是由于 VAAPI 接口被支持的广泛程度远远高于 XvBA。在 openSUSE 系统中该层由 `xvba-video` 软件包实现，在其它的系统中软件包名可能不一样。关于 XvBA 更多的信息可以参看 [维基百科的链接](http://en.wikipedia.org/wiki/X-Video_Bitstream_Acceleration)。

0. VAAPI 是一套提供视频硬件编解码的开源库和标准，它是一种统一的接口，可以让视频解码程序在不同的硬件平台上采用相同的编程接口，无需关心底层实现什么。当前的 Intel 显卡和 AMD 显卡官方采用的都是 VAAPI 接口，而对于 Nvidia 显卡来说，当前也有采用 VAPAU 作为后台的 VAAPI 实现，这算是一种适配技术。VAAPI更详细的介绍请参看 [维基百科的链接](http://en.wikipedia.org/wiki/Video_Acceleration_API)。

0. 在该图中，视频解码程序是指那些支持 VAAPI 编程接口的视频播放程序，像 mplayer-vaapi， mpv， VLC， Kodi（原 XBMC）等都是支持 VAAPI 视频加速的。

在完成上面的视频播放硬件加速解码方案讲解之后，我们接下来要讲解具体的操作步骤了，在继续之前，请核实你的显卡是否满足前面第一条提到的显卡要求，如果不满足，那就不要浪费表情了。

##### 显卡私有驱动安装

在 linux 系统中安装显卡私有驱动，我认为还是应该优先选择社区为你的发行版打好的私有驱动包，其次才是去显卡官方网站下载驱动然后自己编译。这主要还是从降低显卡驱动安装难度这个角度出发，让广大的新手朋友避免驱动编译安装失败的挫折。而在 openSUSE 13.2 系统中，要安装显卡私有驱动，是一件相对容易的事情，唯一需要注意的是，该方法只支持 Radeon HD 5000 系列及以后的显卡，与前文提到的硬件要求相比更加严格了。

0. 下载 [http://geeko.ioda.net/mirror/amd-fglrx/ymp/amd-ati-fglrx64.ymp](http://geeko.ioda.net/mirror/amd-fglrx/ymp/amd-ati-fglrx64.ymp) 文件。
0. 点击该文件即可完成驱动安装。

安装完成之后重启系统，运行 `fglrxinfo` 命令即可查看显卡驱动信息，以此来判断显卡私有驱动是否安装正确；你也可以直接打开 Catalyst Control Center （该工具会出现在你的程序菜单中）来核实。

##### 安装基于XvBA的 VAAPI 实现

VAAPI 接口你无需单独安装，你只需要安装基于XvBA的 VAAPI 实现即可，在 openSUSE 系统中，该实现由`xvba-video` 软件包提供。由于 `xvba-video` 是闭源代码， openSUSE 的官方源是不会提供的，而是由 packman 来进行打包。你只需用浏览器打开 [http://packman.links2linux.org/install/xvba-video](http://packman.links2linux.org/package/xvba-video) 链接，然后点击右上角那个有扳手图标的一键安装链接即可完成安装（对于非 Firefox 浏览器，它会提示你下载一个 ymp 文件，你在下载之后点击 ymp 文件即可）。


##### 安装并配置支持 VAPPI 接口的视频播放器

支持 VAPPI 接口的视频播放器非常多，以我的使用经验来看，mpv 和 VLC 相对来说更容易打开视频加速。mplayer-vaapi 在 openSUSE 下并没有现成的软件包可用，需要自己编译安装，而且 mplayer-vaapi 已有很长一段时间没有代码更新，基本上处于停止开发的状态，不建议使用。我在本文中就只介绍 mpv 了，而且在我的测试中，mpv的硬件加速性能是最好的（[Linux 下 MPV 和 VLC 播放器 VAAPI 显卡加速对比](http://suselinks.us/vlc-mpv-vaapi-gpu-acceleration-comparision-in-linux/)），当然这样的测试并不具有绝对性，毕竟不同的显卡或其它原因都可能引起测试结果的不同。

mpv 软件包也是由 packman 提供的，你只需用浏览器打开 [http://packman.links2linux.org/package/mpv](http://packman.links2linux.org/package/mpv) 链接，然后点击右上角那个有扳手图标的一键安装链接即可完成安装。

现在你剩下的只有配置这个环节了。mpv 默认的编译参数是打开了 VAAPI 支持的，但是却并没有默认用 VAAPI 去进行视频硬件解码和输出。你需要编辑 `～/.config/mpv/mpv.conf` 文件（不存在的话就自行创建），在文件中加入如下内容：

{% highlight text linenos %}
hwdec=vaapi
vo=vaapi,opengl-hq,opengl
{% endhighlight %}

##### 播放视频并验证是否已经开启VAPPI加速

mpv 是命令行程序（当然也做到了与桌面环境集成，可以选中文件然后在右键菜单的打开方式里面进行选择），在终端里面直接运行 `mpv /path/to/yourfile` 即可进行播放。

如何验证播放视频时是否在用 VAAPI 进行视频加速解码呢？ 在终端里运行 `mpv /path/to/yourfile` 命令时，会出来一些像下面这样的日志信息：

{% highlight text linenos %}
[stream] Video (+) --vid=1 (*) (h264)
[stream] Audio (+) --aid=1 --alang=eng (*) (aac)
File tags:
 major_brand: isom
 minor_version: 512
 compatible_brands: isomiso2avc1mp41
 encoder: Lavf54.29.104
libva info: VA-API version 0.34.0
libva info: va_getDriverName() returns 0
libva info: User requested driver 'fglrx'
libva info: Trying to open /usr/lib64/dri/fglrx_drv_video.so
libva info: Found init function __vaDriverInit_0_32
libva info: va_openDriver() returns 0
Trying to use hardware decoding.
AO: [pulse] 44100Hz stereo 2ch float
VO: [vaapi] 1280x720 vaapi
{% endhighlight %}

如果你能看到 `VO: [vaapi] 1280x720 vaapi` 类似的字样（就是末尾那行），那就说明正在用 VAAPI 进行硬件解码。

这里我还要补充一句，并不是所有用 H264 和 VC1 编码的文件都可以进行硬件加速，在视频编码里面有个 `profile` 的概念，只有被显卡和 VAAPI 实现支持的 `profile` 才可以进行。具体有哪些 profile 可以被硬解，可以通过 `vainfo` 命令（在 openSUSE 系统中该命令由 `vaapi-tools` 包提供，也是在 packman 源里面的）来查看：

{% highlight text linenos %}
libva info: VA-API version 0.34.0
libva info: va_getDriverName() returns 0
libva info: User requested driver 'fglrx'
libva info: Trying to open /usr/lib64/dri/fglrx_drv_video.so
libva info: Found init function __vaDriverInit_0_32
libva info: va_openDriver() returns 0
vainfo: VA-API version: 0.34 (libva 1.2.1)
vainfo: Driver version: Splitted-Desktop Systems XvBA backend for VA-API - 0.8.0
vainfo: Supported profile and entrypoints
      VAProfileH264High               : VAEntrypointVLD
      VAProfileVC1Advanced            : VAEntrypointVLD
{% endhighlight %}
