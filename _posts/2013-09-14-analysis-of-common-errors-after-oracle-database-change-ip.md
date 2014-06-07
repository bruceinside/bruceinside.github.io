---
layout: post
title: Oracle 数据库服务器更换IP地址后常见错误分析和处理方法
date: 2013-096-14 01:00:00 +0800
description: ""
tags: [Oracle Database ]
---


Oracle 数据库服务器更换IP地址后，服务端需要修改IP地址的文件包括：

0.  `/etc/hosts`
0.  `$TNS_ADMIN/listener.ora`
0.  `$TNS_ADMIN/tnsnames.ora`

客户端需要修改的文件包括：`tnsnames.ora`。

最佳实践：

0.  不要在 `/etc/hosts` 文件中重复定义主机名，因为 hosts 文件的解析顺序是从上到下的，重复定义可能出现与期望不一致的结果。
0.  最好的做法是在 hosts 文件中定义主机名，在 listener.ora,tnsnames.ora 中使用该主机名

listener.ora 文件中的 host 设置不对时，常见的错误如下:    
尝试执行`sqlplus / as sysdba`会出现下述错误：

{% highlight sql %}
ORA-01031: insufficient privileges
{% endhighlight %}

执行`lsnrctl start`时可能出现下述错误:    
当host设置的IP地址或者主机不存在时，出现如下错误：

{% highlight sql %}
TNS-00515: Connect failed because target host or object does not exist
{% endhighlight %}


当 host 设置的主机A不是本机，但是主机A上面又安装了 oracle 数据库，且监听器没有启动起来的时候，且 *sqlnet.ora中SQLNET.AUTHENTICATION\_SERVICES = (NTS)* 出现下述错误：

{% highlight sql %}
TNS-01189: The listener could not authenticate the user
{% endhighlight %}


当 host 设置的主机A不是本机，但是主机A上面又安装了 oracle 数据库，且监听器已经启动起来的时候，出现下述错误：

{% highlight sql %}
TNS-01106: Listener using listener name LISTENER has already been started
{% endhighlight %}
