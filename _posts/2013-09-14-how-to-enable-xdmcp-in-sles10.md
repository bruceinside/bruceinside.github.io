---
layout: post
title: 在SLES10上配置远程图形登录(xdmcp)
description: ""
tags: [linux, xdmcp, SUSE Linux Enterprise Server]
---


在SLES10上配置远程图形登录(xdmcp)支持需要下面三个步骤：

#####  修改 */etc/sysconfig/displaymanager*
确认 displaymanager 为kdm

{% highlight bash linenos %}
DISPLAYMANAGER="kdm"  #如果你打算使用GDM，则修改为GDM，这也是SLES10的默认值
{% endhighlight %}

将

{% highlight bash linenos %}
DISPLAYMANAGER_REMOTE_ACCESS="no"
{% endhighlight %}

改为

{% highlight bash linenos %}
DISPLAYMANAGER_REMOTE_ACCESS="yes"
{% endhighlight %}

{% highlight bash linenos %}
# 允许root用户远程登录 #
DISPLAYMANAGER_ROOT_LOGIN_REMOTE="yes"
{% endhighlight %}

#####  修改 */etc/opt/kde3/share/config/kdm/kdmrc* 文件的 [Xdmcp] 段

{% highlight bash linenos %}
Enable=false
{% endhighlight %}

改为

{% highlight bash linenos %}
Enable=true
{% endhighlight %}

，如果没有则添上 `Enable=true`
如果你的displaymanager配置为 GDM，则修改 */etc/opt/gnome/gdm/gdm.conf* 文件的 [xdmcp] 段, 将

{% highlight bash linenos %}
Enable=false
{% endhighlight %}

改为

{% highlight bash linenos %}
Enable=true
{% endhighlight %}

#####  启用并重启xdm，在终端下执行下述命令即可。

{% highlight bash linenos %}
insserv xdm
rcxdm restart
{% endhighlight %}
