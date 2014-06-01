---
layout: post
title: 在SLES11上开启远程桌面(XDMCP) 
description: ""
tags: [linux, xdmcp, SUSE Linux Enterprise Server]
---

0.  修改配置文件 */etc/sysconfig/displaymanager*
{% highlight bash linenos %}
## Path:        Desktop/Display manager
## Description: settings to generate a proper displaymanager config

## Type:        string(kdm,kdm3,kdm4,xdm,gdm,wdm,console)
## Default:     ""
#
# Here you can set the default Display manager (kdm/xdm/gdm/wdm/console).
# all changes in this file require a restart of the displaymanager
#
DISPLAYMANAGER="gdm"

## Type:        yesno
## Default:     no
#
# Allow remote access (XDMCP) to your display manager (xdm/kdm/gdm). Please note
# that a modified kdm or xdm configuration, e.g. by KDE control center
# will not be changed. For gdm, values will be updated after change.
# XDMCP service should run only on trusted networks and you have to disable
# firewall for interfaces, where you want to provide this service.
#
DISPLAYMANAGER_REMOTE_ACCESS="yes"

## Type:        yesno
## Default:     no
#
# Allow remote access of the user root to your display manager. Note
# that root can never login if DISPLAYMANAGER_SHUTDOWN is "auto" and
# System/Security/Permissions/PERMISSION_SECURITY is "paranoid"
#
DISPLAYMANAGER_ROOT_LOGIN_REMOTE="yes"
{% endhighlight %}
0.  修改配置文件 */etc/gdm/custom.conf*
{% highlight bash linenos %}
# GDM configuration storage
[xdmcp]
# SuSEconfig: displaymanager:DISPLAYMANAGER_REMOTE_ACCESS
Enable=true
[chooser]
[security]
#DisallowTCP=true
# SuSEconfig: displaymanager:~DISPLAYMANAGER_XSERVER_TCP_PORT_6000_OPEN
DisallowTCP=true
#AllowRemoteRoot=false
# SuSEconfig: displaymanager:DISPLAYMANAGER_ROOT_LOGIN_REMOTE
AllowRemoteRoot=true
[debug]
Enable=true
{% endhighlight %}
0.  重启 *gdm*
{% highlight bash linenos %}
/etc/init.d/xdm restart
{% endhighlight %}


