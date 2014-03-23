---
layout: post
title: 在 openSUSE 上安装 Oracle JDK8 详解
description: "在 openSUSE 上安装 Oracle JDK8，可以和 openJDK 共存和快速切换；支持浏览器运行 Java Applet"
tags: [Java, openSUSE]
---

正与大家都知道的那样，SUSE决定从 openSUSE 12.1开始移除 Oracle Java。这是由于 Oracle 移除了 Java 的操作系统发行商授权导致的，而这早在 Oracle 收购 Sun 公司时我们就已经预见到了。 所以，如果你依旧需要 Oracle JDK，这篇文章将指导你如何安装它。 以安装64位 JDK 为例（32位JDK安装方法请参考 : [在openSUSE 12.2上安装 Oracle(Sun) Java 1.7](http://bruceinside.duapp.com/install-oracle-sun-java-1-7-opensuse-12-2)），步骤如下：

1. #####下载RPM软件包
点击 [Oracle Java 下载页面](http://www.oracle.com/technetwork/java/javase/downloads/index.html)，根据自己操作系统的位数选择对应的RPM包。我下载的是jdk-8-linux-x64.rpm
1. #####安装JDK
可以直接在dolphin中&ldquo;右键单击RPM包-&gt;打开方式-&gt;安装/删除软件&rdquo;进行安装，也可以执行下述命令安装：
{% highlight bash %}
sudo zypper in jdk-8-linux-x64.rpm
{% endhighlight %}
1. #####安装alternatives
安装了java alternative 和javaplugin alternative后，你就可以很方便的在缺省java版本（Iced Tea）和Oracle Java之间切换
{% highlight bash %}
sudo /usr/sbin/update-alternatives --install "/usr/bin/java" "java" "/usr/java/jdk1.8.0/bin/java" 40
sudo /usr/sbin/update-alternatives --install "/usr/lib64/browser-plugins/javaplugin.so" "javaplugin" "/usr/java/jdk1.8.0/jre/lib/amd64/libnpjp2.so" 40
{% endhighlight %}

**注1**:
`update-alternatives --install`对照详解表
| 参数  | 示例值  | 备注
| ----- | ----- | ----
| link（第1个参数） | /usr/bin/java | 符号链接文件路径
| name（第2个参数） | java | alternative名称
| path（第3个参数） | /usr/java/jdk1.8.0/bin/java | 符号链接指向的实际文件路径
| priority（第4个参数） | 40 | 优先级，用于自动模式中

**注2**：
如果你安装的jdk版本和我的不同(一般就是小版本号不同，比如_09)，请酌情替换上述命令中的path（第3个参数）的值。可以通过下面的命令来获取对应的值：
{% highlight bash %}
rpm -ql jdk|grep /bin/java
rpm -ql jdk|grep libnpjp2.so
{% endhighlight %}
1. #####配置alternatives
首先配置java的，执行下述命令：
{% highlight bash %}
sudo /usr/sbin/update-alternatives --config java
{% endhighlight %}
选择和/usr/java/jdk1.8.0/bin/java对应的数字，我这里是1。 接着配置java浏览器插件的，执行下述命令：
{% highlight bash %}
sudo /usr/sbin/update-alternatives --config javaplugin
{% endhighlight %}
选择和 `/usr/java/jdk1.8.0/jre/lib/amd64/libnpjp2.so` 对应的数字，我这里是1。
1. #####验证是否安装成功
  * 先验证java。
  {% highlight bash %}
  java -version
  {% endhighlight %}
  如果该命令的输出信息中应该包含"1.8.0"和&ldquo;HotSpot&rdquo;字样，则说明Oracle JDK8 安装成功了。
   * 再验证Java plugin。
打开浏览器，在地址栏输入 `about:plugins`，你可以看到和下面类似的内容：
![java plugin snapshot](http://i1317.photobucket.com/albums/t638/redhatlinux10/suselinks_us/629356FE2_zpsc47a3a87.png)
   * 默认情况下，Java 8 只允许运行使用了来自可信颁发机构的证书标识的Java应用程序，所以要正常运行下一步的 Java Applet，需要修改下Java的安全级别。运行下述命令:
{% highlight bash %}
/usr/java/jdk1.8.0/bin/ControlPanel
{% endhighlight %}
   * 然后在安全页签调整安全级别为中（默认值是高），如下图所示：
   ![](http://i1317.photobucket.com/albums/t638/redhatlinux10/suselinks_us/629356FE3_zpsa57ab78d.png)
   * 然后访问![www.w3.org网站上的Java Applet测试页面](http://www.w3.org/People/mimasa/test/object/java/Othello)，如果能出现Othello的游戏画面，说明你的java plugin也安装成功了。
**注3**：在google chrome 33, firefox 27 上验证成功。
