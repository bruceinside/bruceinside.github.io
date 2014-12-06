---
layout: post
title: Linux 下 MPV 和 VLC 播放器 VAAPI 显卡加速对比
date: 2014-12-06 10:15:00 +0800
description: "分别在开启 / 不开启 GPU 显卡加速的情况下，对 MPV 和 VLC 视频播放器的 CPU 负载进行测试对比。"
tags: [mpv, VLC, GPU Acceleration, VAAPI]
---

##### 测试环境

###### 硬件环境

:------------- | :-------------
CPU处理器  | AMD 速龙II X2 270
显卡  | 蓝宝石 HD5670 至尊版 512M显存
内存  | 南亚易胜 2G 1333 两条，双通道

###### 软件环境

:------------- | :-------------
操作系统  | openSUSE 13.2 (x86_64) 
桌面环境  | KDE 4.14.3
MPV  | 0.7.1 由 Packman 打包
VLC  | 2.1.5 由 Packman 打包
xvba-video  | 0.8.0-9.1 由 Packman 打包

##### 测试文件

:------------- | :-------------
文件名称  | test.mp4
文件大小  | 851MB
分辨率  | 1920 x 1080
视频时长  | 00:18:32
格式  | H264


##### 测试方法

先不打开显卡硬件加速，分别用 MPV 和 VLC 打开待测试文件，然后用 pidstat 1 40 -p PID 来收集 CPU 负载 ，该语句意思是监控进程号为 PID 的进程，每隔一秒采样一次，共采样 40 次；然后再打开显卡硬件加速，重复上述的测试过程。

##### 测试结果

###### 平均CPU负载对比柱状图

![](http://suselinks-us.qiniudn.com/avg-cpu-usage-comparision-between-mpv-and-vlc-1080p.png)

###### MPV 打开和不打开显卡硬件加速时的平均CPU负载对比

在打开显卡硬件加速的时候，MPV 的平均 CPU 负载为 19.53% ，CPU负载比较稳定，没有出现明显的异常波动。  
在不打开显卡硬件加速的时候，MPV 的平均 CPU 负载为 66.47% ，CPU负载比较稳定，没有出现明显的异常波动。  
点线图如下所示：  
![](http://suselinks-us.qiniudn.com/mpv-with-vs-mpv-without-vaapi-1080p.png)

###### VLC 打开和不打开显卡硬件加速时的平均CPU负载对比

在打开显卡硬件加速的时候，MPV 的平均 CPU 负载为 29.93% ，CPU负载比较稳定，没有出现明显的异常波动。  
在不打开显卡硬件加速的时候，MPV 的平均 CPU 负载为 51.65% ，CPU负载出现了一次明显的异常波动。  
点线图如下所示：  
![](http://suselinks-us.qiniudn.com/vlc-with-vs-vlc-without-vaapi-1080p.png)

###### 打开显卡硬件加速时 VLC 和 MPV 的平均CPU负载对比

在打开显卡硬件加速的时候：  
MPV 的平均 CPU 负载为 19.53% ，CPU负载比较稳定，没有出现明显的异常波动。  
VLC 的平均 CPU 负载为 29.93% ，CPU负载比较稳定，没有出现明显的异常波动。  
点线图如下所示：  
![](http://suselinks-us.qiniudn.com/vlc-vs-mpv-with-vaapi-1080p.png)

###### 不打开显卡硬件加速时 VLC 和 MPV 的平均CPU负载对比

在不打开显卡硬件加速的时候：  
MPV 的平均 CPU 负载为 66.47% ，CPU负载比较稳定，没有出现明显的异常波动。  
VLC 的平均 CPU 负载为 51.65% ，CPU负载出现了一次明显的异常波动。  
点线图如下所示：
![](http://suselinks-us.qiniudn.com/vlc-vs-mpv-without-vaapi-1080p.png)

##### 测试总结

在打开显卡硬件加速的时候，VLC的平均CPU负载高出MPV多达 1/3。  
在不打开显卡硬件加速的时候，VLC的平均CPU负载低于MPV，约1/6。  
VLC出现的异常波动可能是受到测试时系统中其它程序影响，也可能确实是软件解码不稳定所致。  
综上所述，在播放 1080P 甚至更高码率的可硬解视频时，推荐使用MPV。


