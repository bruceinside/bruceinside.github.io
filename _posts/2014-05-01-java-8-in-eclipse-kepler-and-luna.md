---
layout: post
title: 在 Eclipse 中支持 Java 8
description: "指导你如何让你的 Eclipse Kepler 和 Luna 版本支持 Java 8"
tags: [eclipse, kepler, luna, java 8]
---



Java 8 出来已经有段时日了，它是在今年 3 月 18 日正式发布的。 耶! 

但是 Eclipse 作为最大最流行的 Java IDE（至少我这么认为），进度却有点滞后了，到现在为止还没有一个默认支持 Java 8 的 Eclipse 正式版本发布！

虽说如此，我们还是可以在 Eclipse 上进行 Java 8 开发的，因为我前面说的是没有一个**默认支持** Java 8 的 Eclipse **正式版本**。


#### 在 Eclipse Kepler (4.3) 中支持 Java 8
![Eclipse kepler splash screen](http://suselinks-us.qiniudn.com/eclipse-kepler-splash-screen.png)

Kepler (4.3) 是当前最新的发布版本。你可以从 [http://www.eclipse.org/downloads](http://www.eclipse.org/downloads) 下载。默认情况下 Kepler 是不支持 Java 8的。你必须安装一个补丁才行。

0. 下载安装 Java 8（从 Oracle [JRE](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)/[JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) or [OpenJDK](https://jdk8.java.net/download.html)）。如果使用的是 openSUSE 系统，你可以参考这篇博文《[在 openSUSE 上安装 Oracle JDK8 详解](http://suselinks.us/install-oracle-jdk8-on-opensuse/)》。
0. 把它添加到 Eclipse ：
![](http://suselinks-us.qiniudn.com/eclipse-installed-jre.png)
0. 安装补丁。打开 Help → Install New Software...：

~~~
http://download.eclipse.org/eclipse/updates/4.3-P-builds/
~~~

![](http://suselinks-us.qiniudn.com/kepler-install-java8-patch.png)

安装完成之后你需要重启 eclipse 。
![](http://suselinks-us.qiniudn.com/eclipse-java8-test.png)

#### 在 Eclipse Luna (4.4) 中支持 Java 8
![](http://suselinks-us.qiniudn.com/eclipse-luna-splash-screen.png)
Luna (4.4) 是正在开发中的 eclipse 版本。将在这个夏天发布并默认支持 Java 8 。但是你可以下载那些默认支持 Java 8 的开发版本，需要强调的是你必须下载正确的构建版本，因为不是所有的开发构建版本都默认支持 Java 8。
![](http://suselinks-us.qiniudn.com/eclipse-luna-development-build-with-java8-support.png)
顺便提一下，一些开发版本可能包含测试错误的，这是由于 Eclipse Luna 还处于开发中。但是对于学习 Java 8 或者开发小型项目来说，这已经足够了。

#### 简单测试 Java 8
要测试你安装的 Eclipse 版本是否支持 Java 8,这有一个非常简单的代码片段：

~~~java
public class Test {
    public static void main(String[] args) {
        new Thread(() -> System.out.println("Hello Java 8!")).start();
    }
}
~~~

#### 疑难问题解决
如果你的 Eclipse 还是不支持 Java 8,你需要检查你的工程设置。如果工程的兼容级别(compliance level) 没有设置为 1.8，你就用不了 Java 8 了。如果兼容列表里面连 1.8 都没有，那可能极有可能你需要重新下载 Kepler 版本并安装补丁包，或者下载合适的 Luna 开发构建，就如我上面所说的那样。
![](http://suselinks-us.qiniudn.com/eclipse-compliance-level.png)

