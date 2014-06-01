---
layout: post
title: 在 ubuntu 12.04 系统中开启远程桌面（XDMCP）
description: ""
tags: [linux, xdmcp, ubuntu 12.04]
---


要在 *ubuntu 12.04* 中启用 *xdmcp* 服务器是一件相对比较容易的事情，你只需运行命令 `sudo vi /etc/lightdm/lightdm.conf`，并在
*lightdm.conf* 文件末尾添加下述内容即可：

{% highlight bash linenos %}
[XDMCPServer]
enabled=true
{% endhighlight %}

修改完成之后重启系统就可以使用 xmanager 来远程登录了。
但是光这么修改，还不能让 root 用户也能远程登录，你还得执行下述操作:

-   给root用户设置一个密码，激活root用户。在终端中执行：
{% highlight bash %} 
sudo passwd root
{% endhighlight %}

输入当前用户密码，然后输入你要给root用户设置的密码。

-   运行命令 `sudo vi /etc/lightdm/lightdm.conf` ，在文件末尾添加：
{% highlight bash %}
greeter-show-manual-login=true
{% endhighlight %}

现在重启系统，在登录界面，多了一个“登录”选项，你可以输入用户名和密码，到了这一步，你知道怎么做了吧 ^_^.

