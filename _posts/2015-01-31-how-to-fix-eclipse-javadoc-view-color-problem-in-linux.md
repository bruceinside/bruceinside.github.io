---
layout: post
title: linux 系统中 Eclipse 的 JavaDoc 视图颜色问题的解决方法
date: 2015-01-31 15:30:00 +0800
description: "当你在 linux 系统中使用 Eclipse 进行编程开发的时候，你可能会遇到 JavaDoc 视图中文字或者链接颜色和背景色非常相近导致无法看清文字或链接内容的尴尬局面，就像下图中链接文字那样，你得瞪大了眼睛仔细辨认方可勉强看清。"
tags: [Eclipse,linux,openSUSE]
---



##### 什么是 Eclipse 的 JavaDoc 视图颜色问题呢？

当你在 linux 系统中使用 Eclipse 进行编程开发的时候，你可能会遇到 JavaDoc 视图中文字或者链接颜色和背景色非常相近导致无法看清文字或链接内容的尴尬局面，就像下图中**链接文字**那样，你得瞪大了眼睛仔细辨认方可勉强看清。
![](http://suselinks-us.qiniudn.com/eclipse-javadoc-view-color-problem.png)


这就是我在 openSUSE 系统中遇到的问题。在这样蛋疼的颜色对比之下，用不了多久你的眼睛就会吃不消了。

##### 第一时间想到的解决方案

但凡用过 Eclipse 的开发人员应该都知道可以在 Eclipse 中方便的定制颜色和字体 （ *Preferences -> General -> Appearance -> Colors and Fonts* ），在 Java 分类下来也很容易就找到了 *javadoc view background* ,可是当把该颜色设置为其它值时，JavaDoc 视图的背景色却丝毫不变，却是为何？

##### JavaDoc 视图样式和操作系统保持一致

几番搜索，终出结果。

原来 Eclipse 中 JavaDoc 视图的背景色并不能在 Eclipse 中设置，而是只能在操作系统（不论 Windows 还是 Linux）层面设置的！如果你和我一样是 openSUSE 用户，就只需要在 *系统设置 -> 应用程序外观 -> 颜色 -> 颜色* 中修改工具提示背景色（Tooltip Background）和工具提示文字颜色（Tooltip Text）即可。通常我把背景色修改为 `#DCDCDC`（就是四十种颜色中白色左边第一个），文字颜色修改为纯黑色。
![](http://suselinks-us.qiniudn.com/after-fix-eclipse-javadoc-view-color-problem.png)



##### 吐槽

怎么说吧，JavaDoc 视图颜色问题虽然谈不上是个 BUG，但是至少是一个用户体验差的典型代表。同样是和 JavaDoc 视图相关的一个设置，*JavaDoc display font* 就可以直接在 Eclipse 里面设置，你说这能不让人纳闷吗？最起码你给个提示说明啊！

