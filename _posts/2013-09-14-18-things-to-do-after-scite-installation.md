---
layout: post
title: 安装好SciTE文本编辑器之后18件应该做的事情
description: ""
tags: [linux, ubuntu, SciTE]
---


如果你还没有安装好SciTE，可以参考[《在ubuntu 12.04 中安装 SciTE 文本编辑器》]({% post_url 2013-09-14-how-to-install-scite-in-ubuntu-1204 %})。刚安装好的SciTE文本编辑器非常简朴，需要经过适当配置才能成为真正称手的编程利器。下文中的所有配置项，可以直接拷贝，然后粘贴到 SciTE 的用户设置文件中即可生效，部分设置项需要重新启动 SciTE 。在 linux 系统中，SciTE 的用户设置文件为 `~/.SciTEUser.properties`。
你也可以直接下载[我的配置文件](http://ubuntudaily.googlecode.com/files/.SciTEUser.properties "scite user configuration file")，放置在家目录中即可。注意，该文件是隐藏文件，文件前面有一个点号。
下文中自动检测文件编码的相关配置项，需要使用到 *FileDetect.py* 文件，可以点击[这里](http://ubuntudaily.googlecode.com/files/FileDetect.py)，放置到家目录下即可。

##### 修改所有的字体为微软雅黑，如果希望修改为其它字体，只需要修改 *default.font.name* 即可

{% highlight bash linenos %}
default.font.name=font:!Microsoft YaHei
font.base=$(default.font.name),size:11
font.small=$(default.font.name),size:10
font.comment=$(default.font.name),size:11
font.code.comment.box=$(font.comment)
font.code.comment.line=$(font.comment)
font.code.comment.doc=$(font.comment)
font.code.comment.nested=$(font.comment)
font.text=$(default.font.name),size:10
font.text.comment=$(default.font.name),size:9
font.embedded.base=$(default.font.name),size:9
font.embedded.comment=$(default.font.name),size:9
font.monospace=$(default.font.name),size:11
font.vbs=$(default.font.name),size:9  
{% endhighlight %}

##### 修改打开文件窗口的文件过滤器为“全部文件”

{% highlight bash linenos %}
if PLAT_WIN
    all.files=All Files (*.*)|*.*|
    top.filters=$(all.files)|All Source
if PLAT_GTK
    all.files=All Files (*)|*|Hidden Files (.*)|.*|
    top.filters=$(all.files)|All Source
if PLAT_MAC
    all.files=All Files (*.*)|*.*|
    top.filters=$(all.files)All Source
{% endhighlight %}

##### 打开SciTE时默认全屏
{% highlight bash %}
position.maximize=1
{% endhighlight %}

##### 默认显示工具条
{% highlight bash %}
toolbar.visible=1
{% endhighlight %}

##### 让scite工具条使用gnome当前主题提供的图标
{% highlight bash %}
toolbar.usestockicons=1
{% endhighlight %}

##### 默认显示状态栏
{% highlight bash %}
statusbar.visible=1
{% endhighlight %}

##### 在窗口标题栏显示当前文件的全路径文件名称
{% highlight bash %}
title.full.path=1
{% endhighlight %}

##### 默认显示行号
{% highlight bash %}
# 默认显示行号
line.margin.visible=1
# 行号至少占用的宽度
line.margin.width=2+
{% endhighlight %}

##### 高亮当前选中的单词
{% highlight bash %}
# 高亮当前选中的单词
highlight.current.word=1
# 设置高亮单词的颜色
highlight.current.word.colour=#FF0000
{% endhighlight %}

##### 自动重新载入
{% highlight bash %}
# 当前文件被外部程序修改时自动载入
load.on.activate=1
# 自动重新载入前询问
are.you.sure.on.reload=1
{% endhighlight %}

##### 在已运行的SciTE中打开新文件，亦即只允许运行一个SciTE实例
{% highlight bash %}
check.if.already.open=1
{% endhighlight %}

##### 保存最近打开的文件列表
{% highlight bash %}
save.recent=1
{% endhighlight %}

##### 打开SciTE时自动打开上次退出时没有关闭的所有文件
{% highlight bash %}
save.session=1
{% endhighlight %}

##### 使用空格替换tab
{% highlight bash %}
# 设置tab size 为 4个空格的大小
tabsize=4
# 设置缩进的大小为4个空格的大小，可以和tabsize不同
indent.size=4
#缩进时使用空格代替tab，如果设置为1则是空格和tab混合。
use.tabs=0
{% endhighlight %}

##### 自动换行
{% highlight bash %}
# 打开编辑窗格自动换行
wrap=1
# 按字进行换行（更适合亚洲语言）。如果设置为1则是按单词进行换行。
wrap.style=2
# 打开输出窗格自动换行
output.wrap=1
{% endhighlight %}

##### 开启单词自动补全
{% highlight bash %}
autocompleteword.automatic=1
{% endhighlight %}

##### 打开文件时自动检测编码
{% highlight bash %}
# 注：FileDetect.py文件可以通过点击本文开头的相关链接下载
command.discover.properties=python ~/FileDetect.py "$(FilePath)"
{% endhighlight %}

##### xml、html自动闭合括号
{% highlight bash %}
xml.auto.close.tags=1
{% endhighlight %}
