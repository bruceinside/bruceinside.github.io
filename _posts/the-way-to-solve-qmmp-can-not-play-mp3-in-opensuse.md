---
layout: post
title: openSUSE 系统中 Qmmp 不能播放 mp3 的解决方法
date: 2015-01-12 20:30:00 +0800
description: "由于我非常喜欢 qmmp 这款麻雀虽小五脏俱全的音乐播放器，所以在 openSUSE 13.1 安装完成之后，第一时间进行了安装。可是令人奇怪的事发生了，qmmp竟然不能播放 mp3 文件了！"
tags: [packman,mp3,qmmp,openSUSE]
---


注：该文是之前发布在我的博客网站 ubuntudaily.net 上面的博文，由于 ubuntudaily.net 已经不可访问，现在把文章迁移到本站点上来。

由于我非常喜欢 qmmp 这款麻雀虽小五脏俱全的音乐播放器，所以在 openSUSE 13.1
安装完成之后，第一时间进行了安装。可是令人奇怪的事发生了，qmmp竟然不能播放 mp3 文件了！ 这让我非常诧异，我之所以选择使用
qmmp，正是由于它支持的音乐格式非常全面（见我这篇博文《[Qmmp音乐播放器介绍](http://www.suselinks.us/an-
introduction-to-qmmp)》），怎么可能不支持 mp3 格式呢？

##### 解决过程

qmmp 能播放哪些格式的音乐文件，取决于它的输入插件。从 qmmp-&gt;设置-&gt;插件-&gt;decoders 可以看到已经安装了哪些输入插件。
![](http://i2.minus.com/j5Qg8ymXk5lW4.png)

另外，这些输入插件是包含在 libqmmp0-plugins 这个包中的。

{% highlight bash linenos %}
>rpm -ql libqmmp0-plugins
......
/usr/lib64/qmmp/Input
/usr/lib64/qmmp/Input/libcdaudio.so
/usr/lib64/qmmp/Input/libcue.so
/usr/lib64/qmmp/Input/libflac.so
/usr/lib64/qmmp/Input/libgme.so
/usr/lib64/qmmp/Input/libmodplug.so
/usr/lib64/qmmp/Input/libmpc.so
/usr/lib64/qmmp/Input/libsndfile.so
/usr/lib64/qmmp/Input/libvorbis.so
/usr/lib64/qmmp/Input/libwavpack.so
/usr/lib64/qmmp/Output
......
{% endhighlight %}


其实最开始我是不知道哪个插件支持播放 mp3 的，但是有一点我能确定，上面所有已经安装的输入插件都不能播放 mp3。 怎么办呢？我打算从 qmmp
的[官方网站](http://qmmp.ylsoftware.com/)着手，看能不能找到一点线索。最终我在 [http://qmmp.ylsoftware.
com/features.php](http://qmmp.ylsoftware.com/features.php) 找到了点线索。requirements
这个章节提到了 qmmp 依赖于 libmad 和 ffmpeg ，顺瓜摸藤，我找到了这个页面：[http://zh.opensuse.org/openSU
SE:Build_Service_application_blacklist](http://zh.opensuse.org/openSUSE:Build_
Service_application_blacklist)，这个页面明确说明了 openSUSE 出于专利考虑，并不包含 libmad 和
ffmpeg。这意味着，openSUSE的官方源和 OBS 源都不会包含这两个插件啊！结合我上面查看了 libqmmp0-plugins
的内容中确实没有这两个插件， 我立马想到了应该从 packman 源来安装 libqmmp0-plugins，packman
包含了大量多媒体包，而其中有些就是 openSUSE 由于专利问题不能默认自带的。

先看看我当前的包是从哪里安装的。

{% highlight bash linenos %}
&gt; zypper info libqmmp0-plugins
正在加载软件源数据...
正在读取已安装的软件包...
软件包 libqmmp0-plugins 的信息：
--------------------------------
软件源：openSUSE-13.1-Oss-sohu-mirror
名称：libqmmp0-plugins
版本：0.7.2-2.1.4
架构：x86_64
厂商：openSUSE
已安装： 是
状态： 最新
已安装大小：5.1 MiB
摘要：Plugins for libqmmp
描述：
Plugins for libqmmp.
{% endhighlight %}



再从 packman 源重新安装 libqmmp0-plugins。

{% highlight bash linenos %}
sudo zypper ar -f http://packman.inode.at/suse/openSUSE_13.1/ packman
sudo zypper in  --from packman qmmp libqmmp0 libqmmp0-plugins
{% endhighlight %}




安装完成之后，再看看该包的内容：

{% highlight bash linenos %}
&gt; rpm -ql libqmmp0-plugins
......
/usr/lib64/qmmp/Input
/usr/lib64/qmmp/Input/libcdaudio.so
/usr/lib64/qmmp/Input/libcue.so
/usr/lib64/qmmp/Input/libffmpeg.so
/usr/lib64/qmmp/Input/libflac.so
/usr/lib64/qmmp/Input/libgme.so
/usr/lib64/qmmp/Input/libmad.so
/usr/lib64/qmmp/Input/libmodplug.so
/usr/lib64/qmmp/Input/libmpc.so
/usr/lib64/qmmp/Input/libsndfile.so
/usr/lib64/qmmp/Input/libvorbis.so
/usr/lib64/qmmp/Input/libwavpack.so
/usr/lib64/qmmp/Output
......
{% endhighlight %}




啊哈，终于有了！现在重启下 qmmp，是不是可以播放 mp3 了？

##### 总结

1. 要想 qmmp 能播放 mp3 这些受专利保护的编码文件，需要从 packman 安装 libqmmp0-plugins，而不能从 openSUSE官方源或者OBS安装。
2. 经过我的测试， mp3 的解码，其实是由 libmad 这个插件提供的，与 libffmpeg 无关。
