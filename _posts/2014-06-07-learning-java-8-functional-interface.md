---
layout: post
title: 学习 Java 8 - 函数式接口
date: 2014-06-07 22:00:00 +0800
description: ""
tags: [Java, Java 8 ]
---


Java 8 引入了函数式接口的概念。函数式接口其实就是只包含一个抽象方法的普通 Java 接口。在没有引入函数式接口之前，我们通常使用内部类和匿名类来实现类似的功能。而在Java 8 中，我们可以用 lambda 表达式和函数式接口了。和内部类和匿名类比较起来，lambda 表达式的代码更加精简。`java.util.function.Function` 就是一个 Java 8 引入的函数式接口。

##### 如何定义函数式接口

函数式接口可以包含一个拥有任意个参数的方法，函数式接口需要用 `@FunctionalInterface` 来注解。通过这个注解我们可以确保函数式接口只包含一个抽象方法。如果试图添加更多的抽象方法，则会报编译期错误 `Unexpected @FunctionalInterface annotation` 。主要指出的是，函数式接口是还可以包含任意数量的缺省方法（ default method ）和静态方法的。    
没有任何参数的函数式接口    
**Display.java**
{% highlight java linenos %}
package us.suselinks.learningjava8;

@FunctionalInterface
public interface Display {

    void show();
}
{% endhighlight %}


包含一个参数的函数式接口     
**Circle.java**
{% highlight java linenos %}
package us.suselinks.learningjava8;

@FunctionalInterface
public interface Circle {

    double area(double radius);
}
{% endhighlight %}
包含两个参数的函数式接口    
**Adder.java**
{% highlight java linenos %}
package us.suselinks.learningjava8;

@FunctionalInterface
public interface Adder {

    int add(int number1, int number2);
}
{% endhighlight %}

##### 如何使用函数式接口 

现在我们看看如何结合 lambda 表达式来使用函数式接口。下面的示例会实现上面定义的函数式接口。要运行下面的示例，我使用了 JDK8 和 Eclipse Luna 。    
**FunctionalInterfaceDemo.java**
{% highlight java linenos %}
package FunctionalInterfaceDemo;

import us.suselinks.learningjava8.Adder;
import us.suselinks.learningjava8.Circle;
import us.suselinks.learningjava8.Display;

public class FunctionalInterfaceDemo {

    public static void main(String[] args) {
        // 没有任何参数的函数式接口
        Display display = () -> System.out.println("showed up");
        display.show();

        // 包含一个参数的函数式接口
        Circle circle = (num) -> Math.PI * num * num;
        double res = circle.area(5.0);
        System.out.println(res);
        // 包含两个参数的函数式接口
        Adder adder = (int a, int b) -> a + b;
        int rs = adder.add(15, 20);
        System.out.println(rs);

    }
}

{% endhighlight %}
**输出**
{% highlight java  %}
showed up
78.53981633974483
35
{% endhighlight %}


