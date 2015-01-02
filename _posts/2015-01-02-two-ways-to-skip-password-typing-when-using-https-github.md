---
layout: post
title: 使用 GITHUB HTTPS 时两种避免输入密码的方法
date: 2015-01-02 22:30:00 +0800
description: "该文提供了在使用 GITHUB HTTPS 时两种避免输入密码的方法：Cache，Store"
tags: [github, https, credential helper]
---

当你采用 HTTPS 协议向 github 仓库提交代码时，github 每次都会提示你输入用户名和密码，这让人很是抓狂！因为一般情况下我们使用的都是自己的专属电脑，实在没有必要牺牲易用性来保持如此高的信息安全性。幸好 git 提供了一些方法来避免不停输入用户名和密码。

##### 缓存密码到内存

从 git 1.7.10 版本开始，git 提供了一种优雅的机制，让你在使用 HTTP / HTTPS 协议时可以避免不停的输入密码，那就是 [credential helper](http://www.kernel.org/pub/software/scm/git/docs/v1.7.9/gitcredentials.html)，你只需运行命令 `git config --global credential.helper cache`，git 就会把你的密码缓存到内存中，缺省情况下是 15 分钟。你也可以运行命令 `git config --global credential.helper "cache --timeout=3600"` 来自行指定一个超时时间，单位是秒。

如果你使用的 Mac OS 而且使用了 homebrew 来安装 git，则可以运行 `git config --global credential.helper osxkeychain` 来缓存密码。

如果你使用的是 Windows 系统，则运行 `git config --global credential.helper winstore` 来缓存密码。

##### 保存密码到明文文本文件

相比较于缓存密码到内存中，这种方法明显安全性下降了一大截，不过这种方法只需要你输入一次密码，之后哪怕是你重启了操作系统也是有效的，而前者的密码总是会超时的，超时了之后还得输入一次；另外可能在有的系统中缓存到内存的方法不起作用，比如在我的 openSUSE 系统中就不行。

运行 `git config credential.helper store` 就可以永久保存到本地文件（`~/.git-credentials`）中 ，再次强调一下，保存到该文件的用户信息是明文存储的，没有进行任何加密！
