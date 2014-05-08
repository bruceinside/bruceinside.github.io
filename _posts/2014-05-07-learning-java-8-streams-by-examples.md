---
layout: post
title: 学习 Java 8 - Stream 举例说明
date: 2014-05-08 17:30:00 +0800
description: "通过多个代码示例，从多个角度对 Java 8 新引入的 Stream 进行说明"
tags: [Java, Java 8, Stream]
---

在上一篇文章中，我们学习了[内部迭代与外部迭代对比](http://suselinks.us/learning-java-8-internal-vs-external-iteration/)，知道了内部迭代如何帮助我们专注于业务逻辑而无需关心迭代过程，它使得代码更加简洁和具有可读性。在这篇文章中，我们要探讨 Java 8 新引入的另一个概念 - 流（`Stream`） 。
流（`Stream`） 可以定义为从一个源读取而来的支持聚集操作的一系列元素。这里说的源指的是集合或者数组，她们可以为源提供数据。流（`Stream`） 中数据的顺序和源保持一致。聚集操作或者批量操作允许我们很容易对 流（`Stream`） 中的元素进行常见操作。


我将从下面几个方面讨论 Java 8 中的 流（`Stream`），如无特殊说明，下文中的流指的都是 `Stream` 。

* [流 vs. 集合](#streams-vs-collections)
* [各种构建流对象的方式](#different-ways-to-build-streams)
* [转换流为集合](#convert-streams-to-collections)
* [核心的流操作](#core-stream-operations)
  * [中间操作](#intermediate-operations)
  * [末端操作](#terminal-operations)
* [短路操作](#short-circuit-operations)
* [并行处理](#parallelism)

在继续下文之前，我们有必要事先说明， Java 8 中的多数流操作只返回流对象。这有助于写出链式流操作，我们可以管这个叫管线。在这篇文章中我会多次使用这个术语，所以务必记住了哦。

#### <a name="streams-vs-collections"></a>流 vs. 集合

相信大家都在优酷上看过在线视频，当你开始观看视频时，文件的一小部分会首先下载到你的电脑中然后开始播放。在开始播放视频之前你并不需要把整个视频下载下来。我将尝试把这个概念与流关联起来。

在基本概念层面，集合与流的区别在于其中的元素是如何产生的。集合是存在于内存中数据结构，其中保存了所有的元素对象，这些元素对象在加入到集合中之前，必须已经构建好。流本质上不是数据结构，其中的元素对象是按需产生的。这带来了显著的好处，用户只能从流中取到他们真正需要的元素，这些元素也只在用户真正需要的时候被生产出来。这看起来是一种生产者-消费者的关系。

在 Java 中， `java.util.stream.Stream` 代表流，在流上可以执行各种流操作。流操作分为中间操作和末端操作。末端操作会返回某一类型的结果，而中间操作会返回流对象，所以你可以把多个中间操作串成一行。流基于源而生成，比如 `List` 和 `Set` 都可以作为源（`Map` 不行）。流操作可以串行执行，也可以并行执行。

基于上述观点，我们可以总结出来流的基本特征：

* 不是数据结构
* 为 lambda 表达式而设计
* 不支持基于索引的访问方式
* 可以很容易的被转换为集合
* 支持延迟访问
* 可以并行执行

#### <a name="ifferent-ways-to-build-streams"></a>各种构建流对象的方式

下面是最常见的从集合产生流对象的方式

**使用 `Stream.of()`**

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        Stream<String> stream = Stream.of("a", "b", "c", "d", "e", "f", "g");
        stream.forEach(p -> System.out.println(p));
    }
}
{% endhighlight %}

**使用 `Collection.stream()`**

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        List<String> strings = Arrays.asList("a", "b", "c", "d", "e", "f", "g");
        Stream<String> stream = strings.stream();
        stream.forEach(p -> System.out.println(p));
    }
}
{% endhighlight %}

**使用 `generate()`**

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        Stream<Double> stream = Stream.generate(() -> {
            return Math.random();
        }).limit(5);
        stream.forEach(p -> System.out.println(p));
    }
}
{% endhighlight %}

**使用 `iterate()`**

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        Stream<Integer> stream = Stream.iterate(10, i -> i + 10).limit(5);
        stream.forEach(i -> System.out.println(i));
    }
}
{% endhighlight %}

**使用 `CharSequence.chars()`**

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        IntStream stream = "ABCDEFG_abcdefg".chars();
        stream.forEach(p -> System.out.println(p));
    }
}
{% endhighlight %}

还有一些其它的方式也可以产生流对象，比如使用 `java.util.stream.Stream.Builder<T>` 或者使用中间操作。我们会在后续的文章中学习到。

#### <a name="convert-streams-to-collections"></a>转换流为集合

其实不应该说是转换，因为这里并没有发生流对象转换为其它对象的过程，更应该说是把流中的元素放到其它数据结构中，比如集合或者数组。但是如果老是这么描述，有显得太过罗嗦，所以大家务必理解下面我提到的转换。

**使用 `stream.collect(Collectors.toList())` 把 `Stream` 转换为 `List`**

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Stream<Integer> stream = list.stream();
        List<Integer> oddNumbersList = stream.filter(i -> i % 2 != 0).collect(Collectors.toList());
        System.out.print(oddNumbersList);
    }
}
{% endhighlight %}

**使用 `stream.collect(Collectors.toSet())` 把 `Stream` 转换为 `Set`**

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Stream<Integer> stream = list.stream();
        List<Integer> oddNumbersSet = stream.filter(i -> i % 2 != 0).collect(Collectors.toSet());
        System.out.print(oddNumbersSet);
    }
}
{% endhighlight %}

**使用 `stream.toArray(EntryType[]::new)` 把 `Stream` 转换为数组**

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Stream<Integer> stream = list.stream();
        Integer[] oddNumbersArray = stream.filter(i -> i % 2 != 0).toArray(Integer[]::new);
        System.out.print(oddNumbersArray);
    }
}
{% endhighlight %}

还有许多方法可以把 `Stream` 转换为 `Map` 或其它数据类型。你只需要去浏览下 `java.util.stream.Collectors`，把它们记住即可。

#### <a name="core-stream-operations"></a>核心的流操作

你可以在流对象上执行非常多有用的函数。 我并不打算把他们都讲到，但是我计划把最最重要的那些讲一下，这些都是你必须第一时间掌握的。

在继续之前，我们先构建一个字符串集合，下面的例子都会基于这个集合进行操作。

{% highlight java linenos %}
List<String> provinceNames = Arrays.asList("湖南", "湖北", "河南", "河北", "广东", "广西", "北京", "南京");
{% endhighlight %}

这些核心的流操作方法，可以划分为两类：

#### <a name="intermediate-operations"></a>中间操作

中间操作返回流对象，所以你可以把多个中间操作串成一行。
Intermediate operations return the stream itself so you can chain multiple method calls in a row. Let’s learn important ones.

**`filter()`**

filter 方法接受一个断言对象，用来过滤流中的元素。

{% highlight java linenos %}
provinceNames.stream().filter((p) -> p.startsWith("湖")).forEach(p -> System.out.print(p + ", "));

// 输出 : 
// 湖南, 湖北, 
{% endhighlight %}

**map()**

这个中间操作使用给定的函数把流中的每个元素转换为另一个对象。下面的例子把每个字符串转化为另一个字符串（末尾多了一个人字）。但是你也可以把每个对象转换为其它类型的对象。
{% highlight java linenos %}
provinceNames.stream().map(p -> p + "人, ").forEach(p -> System.out.print(p));

// 输出 : 
// 湖南人, 湖北人, 河南人, 河北人, 广东人, 广西人, 北京人, 南京人, 
{% endhighlight %}

**sorted()**

sorted 会对流中的元素进行排序，默认按自然序进行排序，除非你指定一个自定义的比较器（`Comparator`） 。

{% highlight java linenos %}
provinceNames.stream().sorted().map(p -> p + "人, ").forEach(p -> System.out.print(p));

// 输出 : 
// 北京人, 南京人, 广东人, 广西人, 河北人, 河南人, 湖北人, 湖南人, 
{% endhighlight %}

需要注意的是 `sorted` 仅仅对流中的元素进行排序，而不会影响后面的集合，集合中的元素排序保持不变。

#### <a name="terminal-operations"></a>末端操作

末端操作返回某一类型的结果，而不是流对象。

**forEach()**

该方法帮助迭代流中的元素并在元素上执行一些操作。这些操作可以是 lambda 表达式或者方法引用。

{% highlight java linenos %}
provinceNames.stream().forEach(System.out::println);
{% endhighlight %}


**collect()**

collect() 方法用来从流中抽取元素然后保存到一个集合或者数组中。

{% highlight java linenos %}
List<String> provinceNamesOrdered = provinceNames.stream().sorted().collect(Collectors.toList());
System.out.print(provinceNamesOrdered);

// 输出 : 
// [北京, 南京, 广东, 广西, 河北, 河南, 湖北, 湖南]
{% endhighlight %}

**match()**

各种匹配操作用来检查流中的元素是否指定断言条件。所有的匹配操作都是末端操作，它们返回布尔结果。

{% highlight java linenos %}
boolean matchedResult = provinceNames.stream().anyMatch((s) -> s.startsWith("湖"));
System.out.println(matchedResult);

matchedResult = provinceNames.stream().allMatch((s) -> s.startsWith("湖"));
System.out.println(matchedResult);

matchedResult = provinceNames.stream().noneMatch((s) -> s.startsWith("湖"));
System.out.println(matchedResult);

// 输出 : 
// true
// false
// false
{% endhighlight %}

**count()**

count 末端操作返回流中元素个数。

{% highlight java linenos %}
long totalMatched = provinceNames.stream().count();
System.out.println(totalMatched);

// 输出 : 8
{% endhighlight %}

**reduce()**

该末端操作使用给定的函数对流中的元素进行归约。它是这样一个过程：每次迭代，将上一次的迭代结果（第一次时为 identity 的元素，如没有 identity 则为流中的第一个元素）与下一个元素一同执行一个二元函数。在 reduce 操作中，identity 是可选的，如果使用，则作为第一次迭代的第一个元素使用。归约的结果保存在 `Optional` 中。

{% highlight java linenos %}
Optional<String> reduced = provinceNames.stream().reduce((s1, s2) -> s1 + "#" + s2);
reduced.ifPresent(System.out::println);

// 输出 : 
// 湖南#湖北#河南#河北#广东#广西#北京#南京
{% endhighlight %}

#### <a name="short-circuit-operations"></a>短路操作

流操作通常会在流中满足某一断言的所有元素上进行操作，有时我们希望在迭代过程中遇到匹配的元素时就终止操作，在外部迭代中，你需要写 if-else 代码块，在内部迭代中，有现成的方法可以使用，下面是两个这样的方法的示例：

**anyMatch()**
该操作只要遇到满足断言条件的元素就会返回 `true`,在此之后就不再处理任何其它的元素了。

{% highlight java linenos %}
boolean matched = provinceNames.stream().anyMatch((s) -> s.startsWith("河"));
System.out.println(matched);

// 输出 : true
{% endhighlight %}

**findFirst()**

该操作会返回流中的第一个元素，然后就不再处理其它元素了。

{% highlight java linenos %}
String firstMatchedName = provinceNames.stream().filter((s) -> s.startsWith("广")).findFirst().get();
System.out.println(firstMatchedName);

// 输出 : 广东
{% endhighlight %}

#### <a name="parallelism"></a>并行处理
有了 Java 7 中的 Fork/Join 框架，我们有了高效的实现并行操作的机制，但是使用这个框架非常复杂，如果实现不当，那就会引入无数的令人费解的多线程 bug，甚至可能使得应用程序崩溃。有了内部迭代，我们一样可以实现并行操作。 

要启用并行性，你只需创建一个并行流，而不是串行流。这会让你觉得非常惊讶，因为真是太简单了。在上面所有的流操作例子中，任何时候你希望在多核中并行执行你的任务，你只需要改调用 `parallelStream()` 方法而不是 `stream()` 方法。

{% highlight java linenos %}
public class StreamBuilders {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<Integer>();
        for (int i = 1; i < 10; i++) {
            list.add(i);
        }
        // 在这里创建并行流
        Stream<Integer> stream = list.parallelStream();
        Integer[] evenNumbersArr = stream.filter(i -> i % 2 == 0).toArray(Integer[]::new);
        System.out.print(evenNumbersArr);
    }
}
{% endhighlight %}

关于 Java 8 中的 `Stream`， 我就介绍到这了，在后续的文章中我会继续探讨和 `Stream` 相关的话题。