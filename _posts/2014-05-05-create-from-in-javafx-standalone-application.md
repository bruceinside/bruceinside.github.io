---
layout: post
title: JavaFX 独立应用程序开发系列 - 创建表单
date: 2014-05-05 21:30:00 +0800
description: "讲解 hello world 小程序，引导你快速了解如何开发 JavaFX 的独立图形应用程序"
tags: [Java, JavaFX]
---

在开发应用程序时创建表单是非常常见的事情，该教程讲解屏幕布局的基础，如何添加控件到布局面板中，以及如何创建输入事件。
在本教程中，你将使用 JavaFX 创建如图 2-1 所示的登录表单。
图2-1 登录表单
![图2-1 登录表单](http://suselinks-us.qiniudn.com/javafx-login-form.png)

本教程的使用的 JDK8 + Eclipse Luna （支持 Java 8）。在你开始之前，请确保正确安装了 JDK 8 和 Eclipse Luna。
Eclipse Luna 的安装请参考 《[在 Eclipse 中支持 Java 8]({{ site.url }}/java-8-in-eclipse-kepler-and-luna/)》

#### 创建工程
你首先需要在 Eclipse 里面创建一个 Java 工程并命名为 Login:
0. 打开 File 菜单，选择 new → Java Project.
0. 把工程命名为 Login 并点击完成。这里注意一下，务必选择Java 8 作为工程的 JRE。
0. 右击刚刚新建的 Java 工程，选择 new → class, 包名为 test, 类名为 Login 。
0. 打开新建的类，把下述代码复制粘贴进去。
Your first task is to create a JavaFX project in NetBeans IDE and name it Login:

    From the File menu, choose New Project.

    In the JavaFX application category, choose JavaFX Application. Click Next.

    Name the project Login and click Finish.

    When you create a JavaFX project, NetBeans IDE provides a Hello World application as a starting point, which you have already seen if you followed the Hello World tutorial.

    Remove the start() method that NetBeans IDE generated and replace it with the code in Example 2-1.