---
layout: post
title: 学习 Java 8 - Base64 编解码
date: 2014-05-10 13:30:00 +0800
description: "通过代码举例，使用 Java 8 新增的 java.util.Base64 类进行 Base64 编解码。"
tags: [Java, Java 8, Base64, Encoding, Decoding]
---

    
#### Base64 编码是什么？

当你想通过网络传输二进制数据时，你不能仅仅是把你的数据转换成二进制 bit 流然后直接在网络上传输。 为什么呢？ 因为一些媒体格式是只能以文本格式传输的，这是在这些格式设计之初就决定了的，这些协议可能把你的二进制数据中的一些字符当成控制字符来解析，而其实它们根本就不是！

Base64 编码可以把二进制数据转换为可打印的 ASCII 字符，通常被用于 email 消息中的二进制数据编码和 HTTP协议中的 `basic` 认证。 Base64 编码之后的 ASCII 字符串包括 64 个可打印字符，如下：

- 26 个大写字母 [A...Z]
- 26 个小写字母 [a...z]
- 10 个数字 [0...9]
- 2 个其它符号，原始 Base64 中这两个字符是指 '+', '/'， 但是不同的 Base64 实现并不完全相同，请参考[这里](http://en.wikipedia.org/wiki/Base64#Implementations_and_history)

Base64 编码之后的字符串只包含上述字符，是可以在网络上安全传输的，哪怕你的源数据是文本，你无需再担心因为控制字符的混淆而丢失数据。

#### Java 8 之前的 Base64 支持

多年以来， Java 是通过提供非 public 方法来提供 Base64 支持的，不同的 Java 实现可能有的提供该方法，有的则不提供，因此实际上这些方法是不可用的。`java.util.prefs.Base64` 是包可见的，别的包都访问不了， `sun.misc.BASE64Encoder` 没有文档注释，同时也是访问受限的。

#### Java 8 中的 Base64 支持

Java 8 新增了一个类（ `java.util.Base64` ）用于 Base64 编解码。我们通过例子来介绍如何使用它。

**对字符串进行 Base64 编码**

下面是一个简单例子，演示了如何获取一个 `Base64.Encoder` 实例然后使用它来对字符串进行编码。

{% highlight java linenos %}
import java.nio.charset.StandardCharsets;
import java.util.Base64;
import java.util.Base64.Encoder;

public class Base64InJava8 {

    public static void main(String[] args) {
        Encoder encoder = Base64.getEncoder();
        String normalString = "www.suselinks.us";
        String encodedString = encoder.encodeToString(normalString.getBytes(StandardCharsets.UTF_8));
        System.out.println(encodedString);
        
        // output:
        // d3d3LnN1c2VsaW5rcy51cw==

    }
}
{% endhighlight %}

**对字符串进行 Base64 解码**

这也非常简单。你只需获取 `Base64.Decoder` 然后用它来对 Base64 字符串进行解码即可。

{% highlight java linenos %}
import java.util.Base64;
import java.util.Base64.Decoder;

public class Base64InJava8 {

    public static void main(String[] args) {

        String encodedString = "d3d3LnN1c2VsaW5rcy51cw==";
        Decoder decoder = Base64.getDecoder();
        byte[] decodedByteArray = decoder.decode(encodedString);
        System.out.println(new String(decodedByteArray));
        
        // output:
        // www.suselinks.us
    }
}
{% endhighlight %}

**Base64 编码输出流**

如果你不想直接操作数据本身而是想操作输入输出流，你可以对输出流进行包装，这样所有写入该输出流的数据都会被自动进行 Base64 编码。
    
{% highlight java linenos %}
import java.io.IOException;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Base64;

public class Base64InJava8 {

    public static void main(String[] args) throws IOException {

        Path originalPath = Paths.get("/tmp/", "mail.txt");
        Path targetPath = Paths.get("/tmp/", "encoded.txt");
        Base64.Encoder mimeEncoder = Base64.getMimeEncoder();
        try (OutputStream output = Files.newOutputStream(targetPath)) {
            // 拷贝源文件内容，编码，写入目标文件
            Files.copy(originalPath, mimeEncoder.wrap(output));
        }
    }
}
{% endhighlight %}