---
layout: post
title: 在Linux下玩转街机游戏:AdvanceMAME新手指南
description: ""
tags: [linux, AdvanceMAME]
---


![](http://i.imgur.com/Lfbn7Oh.png)

##### 安装 AdvanceMAME（仅提供了openSUSE的安装方法）

打开链接 [在 software.opensuse.org 中搜索 advancemame](http://software.opensuse.org/package/advancemame?search_term=advancemame)，根据实际情况选择 openSUSE 版本然后点击 1 Click Install，然后会弹出Yast2一键安装窗口，之后基本都是直接点击 next 了。
注：有的浏览器会显示 `advancemame.ymp` 的内容而不是自动弹出Yast2一键安装窗口，这时需要你右键另存为 `advancemame.ymp` ，然后双击 `advancemame.ymp` 来启动Yast2一键安装工具。

##### 初始化 AdvanceMAME

在终端运行一次 advmame ,即可完成 AdvanceMAME
的初始化工作，具体来说就是使用缺省选项创建了 AdvanceMAME 的配置文件 `~/.advance/advmame.rc`

##### 下载ROM以及对应的BIOS

在 [http://www.rom-world.com](http://www.rom-world.com/) 网站上收藏了大量适合于MAME模拟器的ROM文件以及BIOS文件。当然如果有需要，大家也可以直接找我索要相关ROM和BIOS。
以三国战记119版本为例，总共需要下载下述三个文件：

{% highlight bash %} 
kovplus.zip
kov.zip
pgm.zip
{% endhighlight %}

其中的 pgm.zip 就是 Poly Game Master的BIOS文件。


再以KOF98为例，总共需要下载下述2个文件：

{% highlight bash %} 
kof98.zip
neogeo.zip
{% endhighlight %}

其中neogeo.zip就是BIOS文件

##### ROM和BIOS放到哪里去？

下载下来的ROM和BIOS文件，需要放到特定目录夹下，AdvanceMAME 才找得到。默认情况下 AdvanceMAME 会依次到 `～/.advance/rom` 和 `/usr/share/advance/rom` 查找 ROM， 当然，我们也可以修改`~/.advance/advmame.rc` 配置文件，使用 'dir_rom' 选项 让 AdvanceMAME 查找其它目录。例如我让 AdvanceMAME 优先查找了 `/mnt/F/Software/game/mame/rom` 目录。

{% highlight bash %} 
dir_rom /mnt/F/Software/game/mame/rom:/home/bruce/.advance/rom:/usr/share/advance/rom
{% endhighlight %}

注1：冒号（:）是目录分隔符。 
注2：直接把zip包放到rom目录下即可，无需解压。

##### 全屏启动游戏

在终端运行

{% highlight bash %} 
advmame 游戏名称
{% endhighlight %}

AdvanceMAME 支持的所有游戏可以通过 `advmame --listxml` 来查看，但是一般情况下，游戏名称就是zip包的名称，以三国战记为例，你只需运行 `advmame kovplus` 即可启动三国战记了。启动后会出来一个白底黑字的界面，显示：

{% highlight bash %} 
省略若干文字......
Otherwise, type OK or move the joystick left then right to continue.
{% endhighlight %}

这时输入OK组合键（先按O键后按K键），接着会出现黑屏，这时连续两次输入OK组合键，即可进入游戏画面。

##### 窗口模式启动游戏

{% highlight bash %} 
advmame -device_video_output window 游戏名称
{% endhighlight %}

##### 设置视频分辨率

进入游戏后，你可能会觉得画面分辨率太低，感觉很粗糙，这时你可以按tab键打开AdvanceMAME的游戏配置界面，选Video,修改 mode 为适合自己的分辨率。在全屏的情况下，我通常选4.00x4.00，在窗口模式下，我通常选2.00x2.00

##### 设置游戏按键

如果你是使用键盘来玩游戏的，那么多半都会觉得默认按键设置不合理。我通常习惯设置AWSD为左上下右移动键，JKUI为功能键（出拳踢脚等）。以设置player1按键为例，设置步骤如下：

0.  按tab键打开AdvanceMAME的游戏配置界面
0.  选中 input(this game) 并进入(按上下箭头方向键移动,回车键确定)
0.  选中 P1 Up并进入编辑状态(按上下箭头方向键移动,回车键确定)
0.  按W键，即可设置成功。（这里会有比较明显的延迟，你要等一下，W字母才会出现在屏幕上）
0.  接下来你有两个选择，一是按ESC键退出按键设置，或者按上下箭头方向键移动光标，然后继续设置其它的。

##### 常用按键介绍

-   数字键5：投币
-   数字键1（不是小键盘上的）：选手一开始
-   数字键2（不是小键盘上的）：选手二开始

##### 截图

拳皇98 
![](http://i.imgur.com/ofuH8GC.png) 

三国战记 
![](http://i.imgur.com/omDUrj4.png)

