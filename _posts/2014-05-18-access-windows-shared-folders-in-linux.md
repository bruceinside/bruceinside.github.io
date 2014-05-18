---
layout: post
title: 在 linux 系统中访问 windows 共享文件夹
date: 2014-05-18 14:30:00 +0800
description: "介绍了 3 种访问 windows 共享文件夹的方法，并分析了它们的优缺点。"
tags: [linux, Gnome, KDE, samba, CIFS, smbclient, mount ]
---


在办公网络、学校宿舍这样的网络环境中， 通过网络邻居共享文件/文件夹是非常常见的事情，在 windows 7 系统中，你右键单击你要共享的文件夹，然后点击共享 -> 高级共享，接着按照下图操作即可完成文件夹共享。
![share a folder in windows 7](http://suselinks-us.qiniudn.com/how-to-share-a-folder-in-windows-7.png)

这样操作完了之后，其它 windows 机器在网络邻居中就可以看到并访问你的共享文件夹了。

但是 linux 操作系统想访问 windows 的共享文件夹，那又该怎么办呢？ 其实 linux 可以非常简单便捷的访问 windows 的共享文件夹，我在这里提供 3 个方法。

#### 在文件管理器中直接访问

利用 `smb` 协议，你可以使用文件管理器直接访问 windows 的共享文件夹。GNOME 桌面中你可以使用 Nautilus ，KDE 桌面中你可以使用 Dolphin 或者 Konqueror 。 你需要在文件管理器的地址栏中输入：

{% highlight bash %}
smb://192.168.8.107/
{% endhighlight %}

你需要把 192.168.8.107 替换为 windows 主机的真实IP地址。 第一次访问时会提示你输入该 windows 主机上的用户名和密码。下图是我在 openSUSE 13.1 的 Dolphin 中访问 windows 7 的共享文件夹。


![access windows shared folder in dolphin using smb protocol](http://suselinks-us.qiniudn.com/access-windows-shared-folder-in-dolphin-using-smb-protocol.png)


尽管在 Nautilus 和 Dolphin 中都是直接在地址栏输入 `smb` 协议的 URI 来进行访问，但是底层的实现是有所区别的。 GNOME 桌面采用 [GVFS](http://en.wikipedia.org/wiki/GVFS)（ `gnome-vfs` 包 ） 这个虚拟文件系统来实现， 由 `gvfs-smb` 包来提供 smb 协议支持。 所以你需要确保这两个包已经安装。 而 KDE 桌面则是由 [KIO](http://en.wikipedia.org/wiki/KIO) （ KDE Input / Output）来支持 smb 访问的。


#### 通过 smbclient 系列命令来访问

smbclient 是一个类似 FTP 的客户端命令，可以访问 SMB/CIFS 资源，windows 的共享文件夹就是用 CIFS 协议实现的，所以 smbclient 一样可以访问 windows 的共享文件夹。当我们想知道 windows 主机共享了那些文件夹时，我们可以这样：

{% highlight bash %}
smbclient -L 192.168.8.107 -U username%password
{% endhighlight %}

`-L` 参数表示列出主机上的共享资源， username 和 password 是 windows 主机上的用户名和密码。

如果你希望像 FTP 客户端那样操作 smbclient, 就输入下述命令：

{% highlight bash %}
smbclient //192.168.8.107//sharedfolder -U username%password
{% endhighlight %}

这样就会出现 `smb: \>` 提示符，进入到 smbclient 环境。 该环境下有许多的命令，比如 `cd`, `lcd`, `ls`, `put`, `get` 等，通过这些命令，我们可以访问远程主机的共享资源。如下图所示：
![smbclient access windows shared folders](http://suselinks-us.qiniudn.com/smbclient-access-windows-shared-folders.png)

#### 挂载共享文件夹

你还可以把 windows 主机的共享文件夹直接挂载到本地目录，然后就像操作本地文件一样操作它们！ 你需要事先创建好挂载点 （ 比如 `/mnt/point` ）,然后在 root 用户下执行下述命令：
{% highlight bash %}
mount -t cifs -o <username>,<password> //<servername>/<sharename> /mnt/point/
{% endhighlight %}

该命令把 *servername* 上的 *sharename* 挂载到本地目录 */mnt/point/* 。如果你想知道更多关于挂载 samba / CIFS 共享文件夹的内容，请看 `man mount.cifs` 。

#### 总结

- 在 GNOME 桌面中，*在文件管理器中直接访问* 就像操作本地文件一样，但是和 *挂载共享文件夹* 比较起来，最大的问题在于， GVFS 是运行在用户空间的，性能比不上直接挂载，因为 CIFS 文件系统是内核内置的，运行于内核空间的。
- 在 KDE 桌面中（注：KDE 4.11.5, mplayer 1.1.1.r37117, Dragon Player 4.11.5），
  0. 同样存在用户空间和内核空间的问题， KIO 是运行于用户空间的。
  0. 但是这还不是最大的麻烦，最大的麻烦在于 *在文件管理器中直接访问* ，如果你想用 mplayer 或者 vlc 打开里面的视频文件，你会发现这两者都打不开，比如 vlc 会报错 `VLC is unable to open the MRL 'smb://......`，这看起来是 KIO 的一个 [BUG](https://bugs.kde.org/show_bug.cgi?id=253547)。
  0. [Dragon Player](http://www.kde.org/applications/multimedia/dragonplayer/) 可以打开 `smb://` 协议的视频文件（估计跟后台使用 Phonon 框架有关），而且可以通过左右方向键进行小幅前进/后退，但是你马上就会发现，当你想通过进度条直接进行大幅的视频进度定位时，需要花费很长的时间去缓冲，仿佛需要把定位点之前的那部分文件内容下载下来才能播放一样，这看起来并不像流文件操作那样可以任意寻址。
- 而如果使用 CIFS 挂载共享文件夹，上述问题就统统不存在了。

所以，我更推荐大家使用 CIFS 挂载共享文件夹。
