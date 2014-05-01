---
layout: post
title: JavaFX 独立应用程序开发系列 - Hello World !
date: 2014-05-01 17:30:00 +0800
description: "讲解 hello world 小程序，引导你快速了解如何开发 JavaFX 的独立图形应用程序"
tags: [Java, JavaFX]
---

JavaFX 是什么我就不赘言了，大家可以参考[Wikipedia 的 JavaFX 词条](http://en.wikipedia.org/wiki/JavaFX)。简而言之，JavaFX 是 Java 用来开发 RIA 应用的，竞争对手应该就是 sliverlight, adobe air, HTML5 等等了；与此同时， JavaFX 还是 Java 用来取代 Swing 的 GUI 工具集 （[Oracle 的官方说法在这里](http://www.oracle.com/technetwork/java/javafx/overview/faq-1446554.html#6)）。

好了，我们切入正题。

要开发和运行 JavaFX 程序，你必须有 JavaFX 的SDK 包。从 JDK 7u6 开始， JavaFX 已经默认包含在标准 JDK 和 JRE 包中了。为简单起见，我强烈建议你安装最新的 Java 8,它默认内置了 JavaFX 8 版本。如何安装 JDK 8,我这里不赘述了，不过如果你使用的是 openSUSE 系统，你倒是可以参考 《[在 openSUSE 上安装 Oracle JDK8 详解]({{ site.url }}/install-oracle-jdk8-on-opensuse/)》。

要跟你讲如何创建一个 JavaFX 应用程序，最好的方法就是写一个 “Hello World” 程序给你看。这篇教程的一个额外好处是可以用来测试你的 JavaFX 是否正确安装了。

本教程的使用的 JDK8 + Eclipse Luna （支持 Java 8）。在你开始之前，请确保正确安装了 JDK 8 和 Eclipse Luna。
Eclipse Luna 的安装请参考 《[在 Eclipse 中支持 Java 8]({{ site.url }}/java-8-in-eclipse-kepler-and-luna/)》

#### 编写应用程序


0. 打开 File 菜单，选择 new → Java Project.
0. 把工程命名为 HelloWorld 并点击完成。这里注意一下，务必选择Java 8 作为工程的 JRE。
0. 右击刚刚新建的 Java 工程，选择 new → class, 包名为 test, 类名为 HelloWorld。
0. 打开新建的类，把下述代码复制粘贴进去。
{% highlight java linenos %}
package test;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class HelloWorld extends Application {
public static void main(String[] args) {
    launch(args);
}

@Override
public void start(Stage primaryStage) {
    primaryStage.setTitle("Hello World!");
    Button btn = new Button();
    btn.setText("Say 'Hello World'");
    btn.setOnAction(new EventHandler<ActionEvent>() {

    @Override
    public void handle(ActionEvent event) {
        System.out.println("Hello World!");
    }
    });

    StackPane root = new StackPane();
    root.getChildren().add(btn);
    primaryStage.setScene(new Scene(root, 300, 250));
    primaryStage.show();
}
}
{% endhighlight %}

下面讲解有关 JavaFX 应用程序基本结构的要点：

JavaFX 应用程序的主类继承了 `javafx.application.Application` 类。`start()` 方法是所有 JavaFX 应用程序的主入口点。

JavaFX 应用程序通过 `Stage` 和 `Scene` 来定义用户界面容器。`Stage` 类是 JavaFX 的顶级容器类。`Scene` 类是所有内容的容器。上述代码创建了 `Scene` ,指定了 `Scene` 的大小。

在 JavaFX 中， scene 的内容表现为许多节点的层次结构图。在我们代码中，根节点是一个 `StackPane` 对象，这是一个可自动调整大小的布局节点。换句话说，根节点的大小会随 scene 的大小而改变（比如当用户改变了 stage的大小）。

根节点只包含了一个子节点，一个有文字的按钮控件，这个按钮绑定了事件处理器，当按下按钮时会打印 *"Hello World!"*。

当你用 JavaFX Packager 打包工具来创建应用程序的 JAR 包时，会把 JavaFX 起动器嵌入到 JAR 包中，在这种情况下，`main()` 方法并不是必须存在于 JavaFX 应用程序中的。但是，如果你要运行不包含 JavaFX 启动器的 JAR 包，那 `main()` 方法就很有用了，比如使用并不完全和 JavaFX 集成的 IDE 工具。而且，嵌入了 JavaFX 代码的Swing程序也需要 `main()` 方法。

下图是 HelloWorld 应用程序的场景图。
![](http://suselinks-us.qiniudn.com/helloworld_scenegraph.png)

#### 运行应用程序
0. 在 Package Explorer 视图中，右击 HelloWorld.java 类，选择 run as -> Java Application
0. 点击 Say Hello World 按钮
0. 在 eclipse 的 Console 视图中看是否打印了 Hello World!

下图是 helloworld 应用的截图：
![](http://suselinks-us.qiniudn.com/helloworld-in-javafx-style.png)

