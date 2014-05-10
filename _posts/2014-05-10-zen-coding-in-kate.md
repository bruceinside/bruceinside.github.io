---
layout: post
title: Kate 中的禅意编程
date: 2014-05-10 15:30:00 +0800
description: ""
tags: [Kate, KDE, Emmet, Zen Coding ]
---


在Kate（开源文本编辑器） 3.11.5 版本中有一项不怎么为人熟知的功能，它允许开发者以迅雷不及掩耳盗铃之势生成 HTML / XML / CSS 代码。该功能是由 [Emmet](http://emmet.io/) 插件提供的。

Emmet ( 即以前的 Zen Coding ) 是一个 Web 开发者工具，可以极大的改进编写 HTML / XML / CSS 的速度。它允许开发者输入类似 CSS 的表达式，比如：

{% highlight css %}
ul#nav>li.item$*4>a{Item $}
{% endhighlight %}

然后动态解析该表达式，把它展开为下述代码：

{% highlight html %}
<ul id="nav">
    <li class="item1"><a href="">Item 1</a></li>
    <li class="item2"><a href="">Item 2</a></li>
    <li class="item3"><a href="">Item 3</a></li>
    <li class="item4"><a href="">Item 4</a></li>
</ul>
{% endhighlight %}

![zen coding in kate](http://suselinks-us.qiniudn.com/zen-coding-in-kate.gif)
要完成这种快速展开，你只需把光标放在该表达式的末尾，然后点击 *工具 → 脚本 → Emmet → 展开缩写* 即可。

通常我会给该功能绑定快捷键为 `CTRL+1` 。绑定可以在 *设置 → 配置快捷键* 里面完成。