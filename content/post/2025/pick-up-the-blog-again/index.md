---

title: "重拾博客"
slug: "again blog"
description: "在新电脑上继续写博客需要做的事情"
date: "2025-06-20T13:56:11+08:00"
lastmod: "2025-10-14T13:56:11+08:00"

categories: ["建站技术"]
tags: ["hugo"]

---

> 2026年3月12日更新，**SMMS**已经更改为[S.EE](https://s.ee/)，账号和已存图片同步迁移，但需要重设密码。

## 引言

换了新电脑，百般无聊之际，突然想到了很久没更新博客了，于是便有了这篇文章。

## 分析

回想一下博客，使用了hugo框架，并且源代码还在我的[GitHub仓库](https://github.com/rento666/My-Blog-By-Hugo-stack)中，所以需要使用**Git**克隆一下项目，然后由于是Hugo框架的博客，所以需要安装一下**Hugo**，图片是存放在[SMMS网站](https://smms.app/)中的，无需下载

## 安装Git

安装git没什么好说的，安装在了D盘，地址如下：[Git安装地址](https://git-scm.com/)，一直默认下一步就行。

### 配置用户名、邮箱
安装完成后，需要配置一下用户名和邮箱。

> 为什么要配置用户名和邮箱？

每次git提交的时候，都会使用这个信息，推荐使用GitHub同款用户名和邮箱。

```
$ git config --global user.name "abc"  
$ git config --global user.email "abc@def.com" 
```

**--global**表示这个命令作用于全局，即所有的项目都会使用这个用户名和邮箱。

### 设置SSH

鼠标右键，点击**Git Bash**，然后输入如下命令：
```
ssh-keygen -t rsa
```

可以一路回车键，也可以按照下图所示，设置了一下存放位置`D:\Programs\Git\.ssh\id_rsa`。(需要自己创建好文件夹`D:\Programs\Git\.ssh`才可以)

![设置SSH，修改存放位置](https://s2.loli.net/2025/06/20/fQ6OHnxcG8Ng3W2.png)

### 配置代理

由于进行clone的时候，可能会由于网络等原因造成操作失败或者速度较慢，所以这里设置了一下代理。

打开`C:\Users\你的用户名`文件夹，找到`.gitconfig`文件，添加如下内容：

```
[http]
    proxy = socks5://127.0.0.1:1234
[https]
    proxy = socks5://127.0.0.1:1234
[git]
    proxy = socks5://127.0.0.1:1234
```

### 绑定Github

打开Github，点击头像，点击`Settings`，点击`SSH and GPG keys`，新增一个SSH key。

![Github SSH key](https://s2.loli.net/2025/06/20/526Xyw3MsOWbNSu.png)

### 安装Git结束

## 安装Hugo

这里采用了`Winget`的安装方法。

### Winget 安装

Winget 是微软为 Windows 提供的官方免费开源软件包管理器。要安装 Hugo 的扩展版：
```
winget install Hugo.Hugo.Extended
``` 

要卸载 Hugo 的扩展版：
```
winget uninstall --name "Hugo (Extended)"
```

### 注意！！

如果您是`Windows`用户：

不要使用`命令提示符(cmd)`

不要使用 `Windows PowerShell`

请在 `PowerShell` 或 `Linux 终端`（如`WSL`或`Git Bash`）中运行这些命令

`PowerShell` 和 `Windows PowerShell` 是不同的应用程序。

如果你又恰好使用了vscode，那么可以这样设置一下：
![VSCODE 设置 Git Bash](https://s2.loli.net/2025/06/20/okz8jQ7C3qYblVN.png)

### 安装Hugo结束

至此，所有的前置操作都已完成，现在可以随意编写博客咯~

---

### 补充一：push到GitHub后博客自动打包

我的项目是放在了**CloudFlare**上，作为一个pages项目，详情可查看[Pages文档 | CloudFlare](https://developers.cloudflare.com/pages/),在“设置-分支控制”启用了自动部署，这样，每当我Push到GitHub上最新的代码后，其就会自动部署，博客就自动出现新的！

## 更换域名

> 2025年10月14日更新

原域名为`caihongtu.asia`，是我在腾讯云买的，首年优惠价12块钱一年，但是后续续费需要80块/年。

但是在玩博客的时候，无意间发现了[哪煮米](https://www.nazhumi.com/)这个域名比价网站，发现[SpaceShip](https://www.spaceship.com/)的top后缀域名最便宜，首年8块钱(1.05刀)，后续续费只需要27块钱左右(3.85刀)

所以，现在本博客的域名便是`caihongtu.top`啦

## 附录

### 参考

- [Github Desktop | Download](https://github.com/apps/desktop)

- [Install Hugo | Doc](https://gohugo.io/installation/windows/)

- [Hugo在Windows下不能在cmd使用](https://hugo.opendocs.io/getting-started/quick-start/#命令)


