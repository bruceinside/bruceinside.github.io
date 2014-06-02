---
layout: post
title: Qmmp音乐播放器介绍
description: ""
tags: [Linux, Qmmp]
---


Qmmp 是一款音乐播放器，采用QT语言编写，用户界面和 winamp,xmms 类似，类 Amarok 和类 foolbar 的用户界面也在开发中。

##### 支持的操作系统：

-   GNU/Linux
-   FreeBSD
-   Microsoft Windows

##### 支持的格式：

-   MPEG1 layer 2/3
-   Ogg Vorbis
-   Native FLAC/Ogg FLAC 无损压缩格式
-   Musepack
-   WavePack
-   tracker modules (mod, s3m, it, xm, etc)
-   ADTS AAC
-   CD 音频
-   WMA, APE格式 (以及其它由FFmpeg库支持的格式)
-   PCM WAVE (以及其它由libsndfile支持的格式)
-   Midi
-   Chiptune formats (AY, GBS, GYM, HES, KSS, NSF, NSFE, SAP, SPC, VGM,
    VGZ, VTX)

##### DSP音效：

-   BS2B effect
-   采样率转换
-   LADSPA effects
-   立体声扩展
-   交叉衰落

##### 视觉特效：

-   ProjectM visualization
-   光谱分析仪

##### 支持的声音输出系统：

-   OSS4 (FreeBSD)
-   ALSA (Linux)
-   Pulse Audio
-   JACK
-   WaveOut (Win32)

##### 额外的特性：

-   支持XMMS and Winamp 2.x皮肤
-   10波段EQ均衡器
-   支持MP3, Vorbis, AAC, AAC+流
-   支持MMS (实验性)
-   支持MPRIS (1.0 and 2.0)
-   可移动设备检测 (通过 HAL or UDisks)
-   视频播放（通过mplayer）
-   歌词 (使用 lyrics.wikia.com)
-   音乐封面
-   支持CUE文件
-   支持FLAC和WavPack格式的内嵌CUE
-   支持多个播放列表，可以在播放列表之间快速切换
-   自动检测CUE文件和ShoutCast元数据的字符编码
-   支持3种播放列表格式: m3u, pls, xspf
-   支持播放增益
-   支持Last.fm/Libre.fm
    scrobbler，可以把电脑中qmmp的播放记录同步到Last.fm/Libre.fm的音乐库中
-   支持CDDB
-   stream browser
-   音频格式转换

##### 让qmmp比其它音乐播放器优秀的关键特性：

-   支持原生FLAC和Ogg FLAC
-   支持APE
-   完美支持CUE文件，支持自动编码检测
-   支持XMMS and Winamp 2.x皮肤

##### 不足之处：

-   默认的歌词插件，歌词来源于国外网站，很多中文歌曲找不到歌词，不能很好的贴合国内用户
-   最新版本不兼容OSD-Lyrics
-   播放列表中不支持搜索

##### 在 openSUSE 系统中安装qmmp：

{% highlight bash %} 
sudo zypper ar http://mirror.pcbeta.com/packman/suse/openSUSE_`lsb_release -r|awk  '{print $2}'`/ packman-pcbeta-mirror
sudo zypper in qmmp
{% endhighlight %}

##### CUE设置：

在qmmp设置 → 插件 → 选中CUE插件 → 参数设置中，包含一些CUE相关的设置项

| 设置项名称 | 含义解释
| ----- | -----
| 自动文件检测 | 如果勾选，则在打开CUE文件时，qmmp会自动关联同名的APE文件，这样就可以在APE文件里的曲目间进行切换了。如果不勾选，则即使打开CUE文件，qmmp的播放列表也会是空的
| 自动检测字符集 | 如果勾选，则不管CUE文件采用什么编码，qmmp都能识别，播放列表不再乱码
| 语言 | 中文用户，请选择zh
| 默认编码 | 中文用户建议选择GB18030,这样即使自动检测字符集失败，一般情况下也不会乱码

##### 用winamp 2.x皮肤装扮qmmp：

qmmp设置 → 外观 → 皮肤 → 添加 →
选择皮肤文件(zip或者wsz文件)，即可完成皮肤安装；安装完成后选中即可切换皮肤。

此外，我收集了许多个人觉得好看的皮肤，先分享几款给大家。


| 预览 | 下载地址 
|:-----:| :-----:
| [![](http://i.imgur.com/h9wFs.png "AlmostWinamp31-0")](http://imgur.com/h9wFs.png)  | [点此下载](http://dl.vmall.com/c09hhsbjb1)
| [![](http://i.imgur.com/7BoJk.png "ECNMY_SEO")](http://imgur.com/7BoJk.png)         | [点此下载](http://dl.vmall.com/c0zoozwuxd) 
| [![](http://i.imgur.com/HVRoy.png "parasiticdecoy1-0")](http://imgur.com/HVRoy.png) | [点此下载](http://dl.vmall.com/c0ltpwprle) 
| [![](http://i.imgur.com/WnMQ1.png "sandwich.brown")](http://imgur.com/WnMQ1.png)    | [点此下载](http://dl.vmall.com/c0v9j6ab41) 



