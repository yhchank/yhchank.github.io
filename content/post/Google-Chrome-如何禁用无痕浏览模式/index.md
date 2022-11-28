---
title: Google Chrome 如何禁用无痕浏览模式
date: 2022-09-04T19:57:19+08:00
tags: [其他]
---

1. 前往谷歌的浏览器策略页面，链接：[https://support.google.com/chrome/a/answer/187202?hl=zh-Hans](https://support.google.com/chrome/a/answer/187202?hl=zh-Hans)

2. 点击`Windows`选项，选择`包含 Google Chrome 模板和文档的 Zip 文件`并下载

   {% asset_image Snipaste_2022-09-04_20-04-32.png %}

3. 打开`policy_templates.zip`，将`windows\admx\chrome.admx`复制到`C:\Windows\PolicyDefinitions\`，并将`windows\admx\zh-CN\chrome.adml`复制到`C:\Windows\PolicyDefinitions\zh-CN\`

   {% asset_image Snipaste_2022-09-04_20-23-29.png %}

   {% asset_image Snipaste_2022-09-04_20-24-13.png %}

4. 启动`gpedit.msc`

   {% asset_image Snipaste_2022-09-04_20-26-11.png %}

5. 打开`计算机配置\管理模板\Google Chrome\`

   {% asset_image Snipaste_2022-09-04_20-27-49.png %}

6. 找到`无痕模式的可用性`，双击打开

   {% asset_image Snipaste_2022-09-04_20-33-04.png %}

7. 选择`已启用`并在下面的菜单中选择`隐身模式已停用`，点击确定

   {% asset_image Snipaste_2022-09-04_20-43-16.png %}

8. 关闭组策略编辑器，现在已经无法启动`无痕浏览模式`

**注：**如需禁用**Microsoft Edge（新版）**的无痕浏览模式，可到[此处](https://www.microsoft.com/zh-cn/edge/business/download)下载策略文件并配置
