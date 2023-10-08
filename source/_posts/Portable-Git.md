---
title: 记录一次使用 Portable Git 遇到的坑
date: 2023-10-08 20:56:11
tags:
  - git
categories:
  - Common
---



由于最近重装了系统，所以准备升级一下 Git。以前在 Windows 中，我一般都使用有安装程序的Git。然而这次去下载的时候，发现了 Portable 版本的，也就是绿色版。一般的Portable 版本软件都是提供一个压缩包的，而 Git 的却是一个 exe 文件 `PortableGit-2.42.0.2-64-bit.7z.exe`。双击安装之后，解压到某个目录，然后把 `git.exe` 添加到 PATH 环境变量就行了。

就在我认为没啥问题的时候，第一个问题就出现了。<!--more-->

## safe.directory

{% asset_img git-safe-dir.png git safe directory %}

根据配置手册里面说的，Git 会拒绝解析其他用户拥有的 Git 仓库。如上图所示，我重装系统之后，硬盘中之前的文件记录的还是之前的用户标识符。
上面的错误提示：把这个仓库路径加入到安全文件夹就可以了。然而这台电脑就我一个人用，而且我的本地 Git 仓库文件夹有很多，不可能一个个去加的，

最好的办法就是使用通配符 `*` 来忽略这个问题。

```shell
git config --global --add safe.directory *
```

## credential

然后就是推送代码的时候，遇到的第二个问题，Git 身份验证器 `CredentialHelperSelector` 一直弹窗。

{% asset_img git-selector.png git credential helper selector %}

我使用的是 `https` 协议来推送代码到 GitHub，为什么不用ssh秘钥呢？主要是因为国内访问GitHub很慢，而且ssh秘钥不能走加速器。

Windows版本的Git内置了一个验证程序，只需要登录一次GitHub，以后就不需要每次都输入密码。然而这个新版本却出了问题，一直弹窗要我选择验证方式，

而且选择了之后，还会再弹窗，于是看了一下Git的credential系统配置。

```shell
git config --system --list

#找到了每次都要求选择的原因了
#....
credential.helper=helper-selector
#....
```

明显验证程序变成了这个选择器，所以每次验证的时候都会调用它, 只需要把 `helper-selector` 换成就 `manager` 可以了。

```shell
git config --system credential.helper manager
```

