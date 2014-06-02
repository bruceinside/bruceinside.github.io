---
layout: post
title: openSUSE12.2 中 Fcitx 输入法随笔
description: ""
tags: [fcitx,openSUSE 12.2]
---


##### 更新Fcitx到最新版本

openSUSE 12.2 默认安装的 Fcitx 版本比较旧了，要安装最新的版本，需要添加[csslayer](http://code.google.com/p/fcitx/)的源，csslayer现在是Fcitx的主力开发者，该源中的Fcitx版本应该是最新的。请执行下述命令：
{% highlight bash %} 
sudo zypper ar http://download.opensuse.org/repositories/home:/csslayer:/fcitx/openSUSE_12.2/ cssplayer-fcitx
sudo zypper in fcitx fcitx-libpinyin fcitx-cloudpinyin fcitx-googlepinyin fcitx-sunpinyin fcitx-table-cn-wubi-large libgooglepinyin0 libsunpinyin3
{% endhighlight %}

在安装的过程中，可能会有类似如下提示：

> 'fcitx'已安装。
> 有一个给 'fcitx' 的更新候选，但它是由一个不同的厂商提供的。使用 'zypper install fcitx-4.2.6.1-51.2.i586' 来安装本更新候选。

照办就是了。

##### 安装fcitx-config-kde4

安装完成 openSUSE 12.2 之后，如果你点击 开始菜单 → 应用程序 → 系统 → 配置 → fcitx 配置工具 ，是会直接使用文本编辑器(比如kwrite)打开 `~/.config/fcitx/config` 文件的。这是由于缺少 `fcitx-config-kde4` 软件包导致的，缺啥补啥：
{% highlight bash %} 
sudo zypper in fcitx-config-kde4
{% endhighlight %}

##### 解决在YaST控制中心搜索输入框中不能输入中文的问题

以 root 权限编辑 `/sbin/yast`：
{% highlight bash %} 
sudo vi /sbin/yast
{% endhighlight %}

在 `export PATH=/sbin:/usr/sbin:$PATH` 下面按“i”键插入：

{% highlight bash %} 
export QT_IM_MODULE=xim GTK_IM_MODULE=xim
{% endhighlight %}

按“ESC”键退出编辑模式，然后按“:wq”保存即可。

##### 快速中英文切换的快捷键

默认情况下，Fcitx的快速中英文切换快捷键是
左CTRL键，我用惯了搜狗输入法，得改成 左SHIFT键。

> Fcitx配置工具 → 全局配置 → 快捷键 → 额外的激活输入法快捷键

需要注意的是， Fcitx配置工具 → 全局配置 → 输出 → 切换状态时提交，这个选项配合快速中英文切换，可以防止已经键入的内容丢失，很有用。

##### 翻页快捷键

默认情况下，逗号(,)句号(.)不是翻页快捷键，和搜狗输入法不一致，得改。我是喜欢这两个键进行翻页的。

##### 自定义短语

在激活Fcitx输入法的情况下，按下分号键则进入快速输入模式。在这种模式下，可以快速输入用户自定义的常用短语或符号。
为了使用该功能，您需要将常用短语和符号按如下格式编辑
{% highlight %} 
<字符组合> <短语>
{% endhighlight %}

并保存为文件 `~/.config/fcitx/data/QuickPhrase.mb`（或把文件放在fcitx的安装目录下的 share/data），一个短语一行。如

{% highlight %} 
zg 中华人民共和国
h http
{% endhighlight %}



