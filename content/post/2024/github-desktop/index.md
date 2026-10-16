---

title: "移动Github Desktop之后，汉化失败的解决办法"
description: GitHub桌面端从C盘移动到其他盘，如何汉化呢？
date: "2024-11-19T11:25:20+08:00"
lastmod: "2024-11-19T11:25:20+08:00"
categories: ["踩坑记录"]
tags: ["github"]

---

## 引言

由于我在安装GitHub Desktop时，它自动安装到了C盘。

## 下载汉化包

对于这个我肯定是不惯着他啊，直接去`C:\Users\%username%\AppData`下的`Local`和`Roaming`目录，将里面的`Github Desktop`和`GithubDesktop`文件夹剪切到了我自己的`D:\Development\Github`文件夹内。

就像下面这个图这样：

![Github目录](https://s2.loli.net/2024/11/19/43h19yEMbDw8sTr.png)

移动文件夹之后，管理员打开CMD，输入：

- 我忘了是Local目录还是Roaming目录的GitHub文件夹有空格了，下面代码你们看自己的目录更改。

```cmd
mklink "C:\Users\%username%\AppData\Local\Github Desktop" "D:\Development\Github\Github Desktop"
mklink "C:\Users\%username%\AppData\Local\GithubDesktop" "D:\Development\Github\GithubDesktop"
```

然后来这个[Github Desktop汉化 - Github仓库](https://github.com/robotze/GithubDesktopZhTool),右侧点击`Releases`下载**对应版本**的汉化工具

## 开始汉化

### 汉化失败

解压之后，点击汉化，发现汉化失败，提示找不着路径。

### 解决办法

打开解压后的汉化包文件夹，进入`Windows`文件夹内，复制其中的`main.js`和`renderer.js`（剪切也可以）

进入`Github\GitHubDesktop\app-3.4.9\resources\app`目录，直接粘贴，替换文件，汉化完成。

如图：

![放这里](https://s2.loli.net/2024/11/19/bnCZkivTFtdy5jL.png)

## 提醒

- 请确保关闭GitHub Desktop 软件再进行汉化！！！
- 请确保软件版本与汉化版本相同！！！

## 附录

### 参考

- [GithubDesktopZhTool - Github](https://github.com/robotze/GithubDesktopZhTool?tab=readme-ov-file)


