---
layout: post
title: 设置 Bing 为 Opera 26 的默认搜索引擎
date: 2014-12-28 14:30:00 +0800
description: "该文指导你如何找回 Opera 的 Bing 搜索引擎并设置为默认搜索引擎。"
tags: [Opera, Bing]
---

Opera 桌面开发团队已经发布了 Opera 26 版本，该版本用 DuckDuckGo 替换了 Bing 搜索引擎，出于安全原因， Opera 不允许用户设置自定义的搜索引擎为默认搜索引擎，所以我建议你就不要创建了，纯属浪费时间。不过你依然可以把 Bing 找回来并设置为 Opera 的默认搜索引擎，只需按照下面的步骤操作即可。

首先关闭 Opera 浏览器，进入 `C:\Program Files (x86)\Opera\26.0.1656.32\resources\` 目录（具体路径在不同的机器上有所不同），然后备份 `default_partner_content.json` 文件为 `default_partner_content.json.bak`。

然后下载 [default_partner_content.json](http://suselinks-us.qiniudn.com/default_partner_content.json)，把该文件复制到 `resources` 目录（就是上文提到的目录）下并覆盖掉原来的文件。

现在启动 Opera，进入 Opera 设置，点击`管理搜索引擎`，你现在可以看到 Bing 出现在默认搜索引擎列表中了，把鼠标悬浮在 Bing 的上面并点击`默认值`，这样就全部完成了。
![](http://suselinks-us.qiniudn.com/set-bing-as-default-search-engine-in-opera-26.png)

如果你希望把 DuckDuckGo 找回来，你需要把之前备份的 `default_partner_content.json.bak` 文件重命名并粘贴到 `resources` 目录下。
