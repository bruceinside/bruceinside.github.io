---
layout: post
title: 如何在 openSUSE 系统中安装支付宝安全控件
description: "描述了在 openSUSE 系统中安装支付宝安全控件的方法，并着重了指出事关成败的关键步骤。"
tags: [openSUSE, alipay, 支付宝安全控件]
---

网购未动，控件先行。

>支付宝安全控件会时时保护您的密码及账号不被窃取，从而有效的保障您的账户资金安全。当您在电脑上进行交易时，安全控件会及时的发现风险并提醒您，有效制止仿冒网站的交易欺诈。

linux 系统中的支付宝安全控件，只适用于firefox，chrome，opera浏览器，切记。

其实在 openSUSE 中安装支付宝安全控件是一件非常简单的事情，只是其中有那么一个环节，如果你不知道，那就可能让你卡壳，抓破头也不知道怎么到自己手中就不对劲了呢？

下面我以火狐浏览器为例，介绍下安装步骤：

- 首先用火狐浏览器打开[支付宝首页](https://www.alipay.com/),你可以看到密码输入框提示你“请点此安装控件”。
![支付宝登录页面局部截图](http://suselinks-us.qiniudn.com/alipay-login-page-snapshot.png)

- 然后你点击并下载一个名叫 `aliedit.tar.gz` 的文件，接着在文件管理器(比如 Dolphin )里双击该文件，可以从 `aliedit.tar.gz` 文件里面解压出来一个脚本文件 `aliedit.sh`，放到你的家目录下。
- 打开终端(比如 konsole)，运行下述命令：
  {% highlight bash %}
  sh ~/aliedit.sh
  {% endhighlight %}
- 最可能让你卡壳的一步来了，之所以这么说，是因为支付宝官方并没有说明支付宝安全控件到底有哪些依赖。这会让你以为跑完上面的脚本就万事大吉了。事实上安全控件依赖了 `libpng12-0`，而这个包恰恰是 openSUSE 默认没有安装的。运行下述命令进行安装：
  {% highlight bash %}
  sudo zypper in libpng12-0
  {% endhighlight %}

好了，现在重新打开火狐浏览器并访问[支付宝首页](https://www.alipay.com/)，密码输入框是不是提示你，是否允许运行该控件？ 允许就是了，自此开启 openSUSE 上的淘宝之旅！



