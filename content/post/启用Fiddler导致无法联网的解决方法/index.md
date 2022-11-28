---
title: 启用Fiddler导致无法联网的解决方法
date: 2022-08-30T21:45:49+08:00
tags: [其他]
---

**解决方法：**

1. 打开`fiddler`-`tools`-`options`-`https`

2. 取消勾选`decrypt https traffic`

   ![](image-20220830085929624.png)

3. 点击`actions-remove interception certficates` ，删除所有证书

   ![](image-20220830085915971.png)

4. 重新勾选`decrypt https traffic`，根据提示安装证书

   ![](image-20220830090534349.png)
