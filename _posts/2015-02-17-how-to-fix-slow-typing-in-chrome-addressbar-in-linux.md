---
layout: post
title: linux 系统中 Chrome 地址栏键入非常缓慢的解决方法
date: 2015-02-17 02:00:00 +0800
description: "当我在 openSUSE 系统中使用 Google Chrome ( V41 ) 浏览器时，会发现有这么几个性能问题存在：在地址栏输入内容时，发现键入速度非常缓慢......"
tags: [Google Chrome,linux,Address Bar, Omnibox]
---



##### 遇到了什么问题？

当我在 openSUSE 系统中使用 Google Chrome ( V41 ) 浏览器时，会发现有这么几个性能问题存在：
0. 在地址栏输入内容时，发现键入速度非常缓慢，完全赶不上我敲击键盘按键的速度，有时还会出现键入的字符乱序的情况。当把光标放在地址栏URL的末尾，然后按 Backspace 键进行删除时，也非常的缓慢。
0. 切换浏览页签也能感觉到明显的延迟。
0. 点击右上角的三道杠图标来打开 Google Chrome 的菜单，也能感觉到菜单弹出有延迟，同时在菜单上移动鼠标，一样能感觉到菜单项的切换有延迟。
0. 执行上述操作时可以观察到 Google Chrome 的 UI 进程的 CPU 占用率会狂升至 100% 左右！

这4点让我非常抓狂，一度让我弃用 Google Chrome，转向 Opera(一样是基于 Chromium 的)，无奈 Opera 一样不争气，虽然情况稍有改善，却依旧难以忍受。看起来这是 Chromium 的问题，非 Google Chrome 独有。

另外根据社区的反馈，该问题也不只是在 openSUSE 中存在，而是一个 linux 下都存在的问题。


##### 一线生机

在 Chromium 的故障跟踪系统中有这么个故障：[Omnibox popup is slow to paint on Linux & ChromeOS when many suggestions are present](https://code.google.com/p/chromium/issues/detail?id=376077)，描述的现象和我遇到的一模一样。从该故障的描述和回复来看，可以总结出：

0. 该现象从 Google Chrome 35 版本开始出现。
0. 该故障和 Aura UI 有关，而 Aura UI 正是从 35版本开始引入的。
0. 启动 Google Chrome 时带上 `--enable-harfbuzz-rendertext` 参数之后，可以启动 `RenderTextHarfbuzz` 来进行 UI 文本渲染，在地址栏的键入速度就会变快了。

看到这里，我已经抑制不住激动的心情，在 Google Chrome 的启动快捷方式里面火速加入 `--enable-harfbuzz-rendertext` 参数，重启 Google Chrome，双手张开，准备拥抱胜利的果实。

然而问题依旧......

为什么呢？再仔细阅读上述故障，[comment 27](https://code.google.com/p/chromium/issues/detail?id=376077#c27) 提到了：

>I'm hoping we can enable RenderTextHarfbuzz by default this week, now that font fallback landed.
That still may not fix this issue, since RenderTextHarfBuzz::ShapeRun calls internal::CreateSkiaTypeface.

>I don't think we ought to cache stylized fonts within a RenderText object, that probably wouldn't help either.
(in this scenario, the omnibox dropdown result views are being tossed and recreated as fast as possible)

>It seems like we'd need a centralized cache of fonts, so the most commonly used ones are readily available.
Perhaps in the near term, OmniboxResultView could cache stylized FontLists for its RenderText instances.

原来问题的根源出在 Skia 的字体这块，然而由于 `RenderTextHarfBuzz` 还是调用了 `internal::CreateSkiaTypeface`，所以问题还是可能存在。

##### 转折

有方向了，Skia 的字体问题和 `CreateSkiaTypeface` API！

再一次，又找到一个故障： [Excessive call for SkFontConfigInterfaceDirect::matchFamilyName from OmniboxResultView](https://code.google.com/p/chromium/issues/detail?id=424082)

故障描述中说，为了渲染地址栏的弹出菜单，每当你键入一个字符， `matchFamilyName()` 方法就会被调用 100 到 200 次！ 同时 [comment 30](https://code.google.com/p/chromium/issues/detail?id=424082#c30) 给出了真正的问题根源：

0. Google Chrome UI 的缺省字体继承自 Gnome 桌面设置（而不是 `chrome://settings/`）。
0. 地址栏弹出框的缺省字体也是继承自 Gnome 桌面设置。
0. 字体名称的官方名称和本地化名称不相同导致了 Skia 缓存无法命中。
0. Skia 缓存无法命中导致 fontconfig 频繁被调用，而该调用非常消耗 CPU 时间！


##### 结论

0. 啊哈！问题根源找到了！ 该 comment 的作者也提交补丁了，可是 Google Chrome 41 版本还没有打该补丁的呀，怎么办呢？ 上述故障的末尾也提到了一个 workaround，那就是修改 Gnome 桌面的缺省字体。以使用 KDE 作为默认桌面的系统来说，打开 *系统设置 -> 应用程序外观 -> GTK* ，修改字体（默认是 *无衬线*）为 *Droid Sans* 或者 *DejaVu Sans*，反正字体名称不是中文的就行（比如 *文泉驿微米黑* 这种有本地化字体名称的就不行）。
0. 该补丁可能会合入 42 版本，但是根据我的测试，*google-chrome-unstable-42.0.2298.0-1.x86_64* 尚未包含该补丁。
0. 对于 40 版本以前的 Google Chrome ，你还需要在启动 Google Chrome 时带上 `--enable-harfbuzz-rendertext` 参数。
0. 做了上述修改之后，该文开头提到了4个性能问题都解决了！哦耶！
