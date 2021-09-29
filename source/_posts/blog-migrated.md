---
title: 本网站已迁移到腾讯云静态托管
date: 2021-04-29 17:26:54
tags: 
 - GitHub
 - Action
 - Hexo
categories:
  - Common
---
本网站现在已经迁移到腾讯云静态托管了,现在博客源码存放于Github,
使用`Github Action` 来进行同步,当我把提交推送到Github时,就会触发Action的工作流。

工作流主要做两件事

- 调用`hexo generate`生成静态页面文件

- 同步静态文件到腾讯云静态托管服务器。

  

你如果在我的网站上看到了这篇文章，那说明工作流已经部署成功了。
