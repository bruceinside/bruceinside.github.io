---
layout: post
title: 学习 Java 8 - 内部迭代与外部迭代对比
date: 2014-05-06 21:30:00 +0800
description: "通过与 Java 之前的迭代方式进行对比，介绍了 Java 8 新引入了内部迭代的用法及其优点。"
tags: [Java, Java 8, interal iteration, 内部迭代]
---


#### 外部迭代

直到 Java 7, 容器框架还是依赖于外部迭代的。什么是外部迭代呢？ 这么来说， `Collection` 通过实现 `Iterable` 接口，提供了一种枚举容器中元素的方法。通过使用 `Iterator`， 我们可以依次遍历容器中的元素。例如，如果我们想把所有字符串变为大写形式，我们可以这么写：

{% highlight java linenos %}
public class IterationExamples {
    public static void main(String[] args) {
        List<String> alphabets = Arrays.asList(new String[] { "a", "b", "b", "d" });
        for (String letter : alphabets) {
            System.out.println(letter.toUpperCase());
        }
    }
}
{% endhighlight %}

或者我们也可以这样写：
{% highlight java linenos %}
public class IterationExamples {
    public static void main(String[] args) {
        List<String> alphabets = Arrays.asList(new String[] { "a", "b", "b", "d" });
        Iterator<String> iterator = alphabets.listIterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next().toUpperCase());
        }
    }
}
{% endhighlight %}

上面的两个代码片段用的都是外部迭代。外部迭代已经非常简单了，但是还有几个不足：
0. Java 的 for-each 循环/迭代本质上是有序的，它必须按照容器指定的顺序处理元素。
0. 它限制了 Java 管理流程的可能性，而这种可能性可以通过对数据进行重新排序、并行处理、短路、延迟处理来提供更好的性能。


#### 内部迭代

在一些情况下，我们是希望 for-each 循环保证有序性的。但是这往往降低了性能。与外部迭代相对应的是内部迭代，内部迭代不再需要用户自己控制迭代，而是把它交给 JVM，用户只需提供要在元素上执行的代码即可。

与上述示例等价的内部迭代代码如下：
{% highlight java linenos %}
public class IterationExamples {
    public static void main(String[] args) {
        List<String> alphabets = Arrays.asList(new String[] { "a", "b", "b", "d" });
        alphabets.forEach(l -> System.out.println(l.toUpperCase()));
    }
}
{% endhighlight %}

外部迭代把“做什么”( 转换为大写 ) 与 “怎么做”( 循环/迭代 ) 混淆在了一起，内部迭代只需用户提供“做什么”，而把“怎么做”交给了JVM。这提供了一些潜在益处：
0. 用户代码只需关注解决问题，无需关注如何解决的细节，从而变得更加清晰了。
0. 内部迭代使得 JVM 利用短路、并行处理和乱序执行来提升性能成为可能（JVM是否利用了这些来优化性能取决于JVM实现本身，但是有了内部迭代这些至少是可能的，而对于外部迭代来说则是不可能的）。