---
title: "[全流程] 在笔记本上安装Ubuntu22.04"
slug: "ubuntu"
description: "本文详细介绍了下载ISO镜像文件、制作系统U盘、调试BIOS到最终成功启动Ubuntu的全流程。"
date: "2026-08-18 14:33:56 +0800"
categories: ["踩坑记录"]
tags: ["Ubuntu"]
---

{{<link title="Windows11安装Ubuntu避坑指南 | 哔哩哔哩" link="https://www.bilibili.com/video/BV1Cc41127B9?p=1" cover="auto">}}

> [!TIP]
> 由于旧笔记本的配置是R7-4800H + 2060，询问AI后得知可能不是很适配Ubuntu26.04，所以便选择了22.04这一版本。如果你的硬件较新（例如是50系显卡），可以选择下载最新的版本。

> - Ubuntu各个版本的安装镜像文件可以从华为云下载。<br>
> - Ubuntu桌面版的安装镜像文件名为：`ubuntu-<版本号>-desktop-amd64.iso`

{{<link title="Ubuntu | 华为云" link="http://repo.huaweicloud.com/ubuntu-releases/" cover="auto">}}

![Ubuntu22.04](https://files.seeusercontent.com/2026/08/18/2Pmo/Ubuntu1.png)

## 1.查看引导方式

Ubuntu需要与Windows保存相同的**引导方式**才能使用双系统菜单。

### BIOS模式

`win + r`打开运行，输入`msinfo32`，确定，就打开了系统信息页面。

![BIOS模式](https://files.seeusercontent.com/2026/08/18/Q0da/2026-08-18-083247.png)

我们需要关注这个**BIOS模式**，一般近些年的电脑都是`UEFI`。

> [!WARNING]
> 如果显示的是`传统`，那说明电脑有了些年份，本次教程**可能不适用**。

### 硬盘分区形式

我们搜索**磁盘管理**，然后打开。

![磁盘管理](https://files.seeusercontent.com/2026/08/18/7Dbu/20260818164024323.png)

找到`C盘`所使用的那个磁盘，如下图所示，是磁盘0，右键，点击`属性`。

![磁盘属性](https://files.seeusercontent.com/2026/08/18/5Ubd/2026-08-18-084543.png)

点击`卷`分类，看一下`磁盘分区形式`是否为`GUID分区表(GPT)`。

![GPT](https://files.seeusercontent.com/2026/08/18/Rnl8/2026-08-18-085134.png)

简单来说，**分区形式（MBR 或 GPT）必须与电脑的启动模式（Legacy/BIOS 或 UEFI）相匹配**。其中，`GPT` 通常对应较新的 `UEFI` 模式，而 `MBR` 对应传统的 `Legacy/BIOS` 模式。

如果是`MBR`，建议转成`GPT`，否则本次教程**可能不适用**。

## 2.制作系统U盘

> [!NOTE]
> - 需要准备一个储存空间大于8G的U盘或移动硬盘（16G、32G及以上均可）<br>
> - 制作系统U盘有较低的风险损坏U盘（尤其是杂牌或者赠品U盘）
> - **开始之前要把U盘数据备份！！！**（制作过程中会将U盘内容全部清空！！！）

### 下载Ventoy

**开始之前要把U盘数据备份！！！**

**开始之前要把U盘数据备份！！！**

**开始之前要把U盘数据备份！！！**

**重要的事情说三遍！！！**

进入[Ventoy下载页](https://www.ventoy.net/cn/download.html)，往下拉找到`说明`这一项。

![Ventoy说明](https://files.seeusercontent.com/2026/08/18/0aUa/20260818171325773.png)

从中选择一个下载，这里我们选择[山东大学镜像站](https://mirrors.sdu.edu.cn/github-release/ventoy_Ventoy)。

我们只需要下载`Ventoy-1.1.17.windows.zip`这个就行，其中`1.1.17`是我写这篇文章时的版本号，你只需要下载最新的就行。

下载的是zip压缩包，我们需要解压出来，解压过程就不多解释，不会的可以去文章开头点击B站视频去详细看看。

解压出来之后，先把U盘插入到电脑的USB口。

识别到U盘后，我们打开`Ventoy2Disk.exe`文件。

![](https://files.seeusercontent.com/2026/08/18/B4um/2026-08-18-091815.png)

确定好识别的U盘设备是正确的。如下图，我的是E盘盘符、32GB容量、金士顿3.0的U盘。

（在未安装时，右侧**设备内部 Ventoy 版本是空的**！）

![](https://files.seeusercontent.com/2026/08/18/q1Io/2026-08-18-092525.png)

初次打开，左侧可能是显示`MBR`，并非`GPT`，需要我们依次点击`配置选项`-`分区类型`，选择`GPT`。

![](https://files.seeusercontent.com/2026/08/18/Gmx7/2026-08-18-092139.png)

无误后，点击`安装`即可，完成后就能关闭啦。

### 拷贝ISO文件到U盘

将之前下载好的ISO文件[<sup>跳转到下载地址</sup>](#anchor-ubuntu)复制到U盘中，如下图。(图中多了个Win11，忽略即可)

![](https://files.seeusercontent.com/2026/08/18/E9me/2026-08-18-093522.png)

## 3.关闭BitLocker

还记得[硬盘分区形式](#硬盘分区形式)的步骤吗？我们打开磁盘管理，如果里面有显示**BitLocker已加密**，那么就需要关闭，反之则跳过本小节。如下图，是上面那种则需要关闭。

![bitlocker](https://files.seeusercontent.com/2026/08/18/F9mv/bitlocker.png)

直接搜索**设备加密**，然后点进去关闭就行啦。（有两种形式，如下图）

![关闭设备加密](https://files.seeusercontent.com/2026/08/18/1Mqp/20260818183211547.png)

![关闭设备加密2](https://files.seeusercontent.com/2026/08/18/4ulA/20260818183333838.png)

## 4.关闭独显直连（笔记本特有）

首先去网上查，你的笔记本电脑的CPU是否带有核显，如果没有核显，则跳过本小节。

如果你的CPU有核显，并且`任务管理器-性能`中，能够找到两个GPU，分别是GPU0和GPU1，那么也可以跳过本小节。

![两个GPU](https://files.seeusercontent.com/2026/08/18/k1Vp/2026-08-18-105408.png)

**如果你的CPU有核显，但是在任务管理器中只发现了一种GPU**，那么就要搜索你电脑品牌的独显直连怎么关闭了。

这个与笔记本品牌有关，联想、机械革命等等电脑的方式均不相同。

一般地，我们设置成混合模式就行。

## 5.为Ubuntu整理出磁盘空间

> [!WARNING]
> 在开始之前，请确保要操作的硬盘中**没有未备份的重要资料**。

安装之前，我们需要挤出一些硬盘空间来为Ubuntu服务。毕竟没有空间的话，Ubuntu装哪里呢？对吧。总不能装到Windows系统中吧！

与[硬盘分区形式](#硬盘分区形式)的步骤一致，我们打开`磁盘管理`页面。

> [!NOTE]
> 这里先介绍两个概念：<br>
> 压缩卷：从已有分区中“挤出”一部分空闲空间，变为未分配区域。原有的分区和里面的数据都还在，只是容量变小了。<br>
> 删除卷：将整个分区变为未分配区域。（数据全部清空）

所以说，如果你有两块硬盘，希望一个装Windows，另一个装Linux，那么可以使用**删除卷**，将该硬盘转化为未分配区域的空盘，将其从Windows系统中去掉，这样就可以用来装Linux了。

但如果你想把Linux与Windows放到同一个硬盘上，那么就可以使用**压缩卷**，从空间较多的盘中挤出一部分用来装Linux。

![](https://files.seeusercontent.com/2026/08/18/tt8U/E1C6813B-B9DF-4431-824A-7E0F0C40.png)

如上图所示，如果我们要对E盘进行改动，可以有两种方式：

1. 将E盘划分出100G来作为Linux的磁盘空间。**使用“压缩卷”。**

2. 将E盘数据清空，作为Linux系统盘。**使用“删除卷”。**

如果使用“压缩卷”时，请注意，可能会出现如下图这种情况：

**可用压缩空间大小 远远低于 可用空间大小**

![](https://files.seeusercontent.com/2026/08/18/xpF3/20260818192119358.png)

我们可以使用diskgenius[<sup>下载地址</sup>](#anchor-diskgenius)这个分区工具来进行分区。

下载、解压、打开之后呢，我们可以点击“分析”，可以看到这个磁盘的使用情况。

![E盘使用情况](https://files.seeusercontent.com/2026/08/18/nT8v/20260818194243313.png)

可以看到，蓝色部分为已分配，白色部分为空闲。这个蓝色的不是连续的，而可分配的空间只会从末尾去找，这就导致了中间有较多的空闲空间不能分配。

### 使用diskgenius调整

**右键**点击要操作的盘(我这里操作的是E盘)，然后选择**调整分区大小**。

![调整分区大小2](https://files.seeusercontent.com/2026/08/18/6yiD/6DF17C70-4D72-4A69-AF87-34AA81D0.png)

打开之后，鼠标按住**彩色条的最右侧边界**，向左拖动，拖到你想要的大小即可。此时，右侧灰色区域就是即将释放出来的“未分配空间”。**如下图红箭头所指位置。**

![调整3](https://files.seeusercontent.com/2026/08/18/o2Vy/20260818195455386.png)

或者说，我们也可以在**分区后部的空间**这里输入`100.00GB`，如下图所示。

![调整4](https://files.seeusercontent.com/2026/08/18/qN5r/20260818195802152.png)

#### 之后就**点击“开始”并执行**。

点击窗口下方的 “开始” 按钮。

软件会弹出一个警告提示，告诉你操作涉及系统关键文件，点击“是” 确认。

因为E盘正在被Windows占用（里面有页面文件或元数据），DiskGenius会提示：“需要重启计算机才能执行此操作”。

点击 “确定” 或 “立即重启”。

#### **等待电脑自动完成（切勿断电！）**

电脑重启后，**不会**正常进入Windows桌面，而是会进入一个类似黑底蓝白字的DOS界面（这是DiskGenius的预执行环境）。

**请千万不要按任何按键**，耐心等待它自动跑完进度条（通常几分钟）。

跑完后，电脑会自动再次重启，正常进入Windows。

#### 操作完成后的检查

进入Windows后，再次打开“此电脑”，你会发现E盘的容量变小了，多出来的那部分在DiskGenius里显示为 “空闲” 或 “未分配” 的灰色条块。

到此，磁盘分区我们已经完成了。

## 6.操作BIOS

首先贴上各大品牌的BIOS启动按键。

![BIOS按键](https://files.seeusercontent.com/2026/08/18/Mvp7/20260818200506544.png)

通常是 `F2`、`F12`、`Esc` 或 `Del` 键。

**首先**，我们将U盘插入电脑的USB口，然后开机。

在屏幕还没出现画面之前，狂按启动按键，不停的按，直到屏幕出现画面。

一般地，如果你按的启动按键是正确的，那么会出现品牌LOGO + 下面小字。如图

![BIOS](https://files.seeusercontent.com/2026/08/18/wQo8/20260818201114310.png)

此时我们按回车键进入BIOS面板。

1. `Security` → `Secure Boot：Disabled`（关闭安全启动）。
2. `Advanced` → `Switchable Graphics`：设置为 `MsHybrid`（混合模式），**不要 dGPUOnly 独显直连！！**[<sup>部分笔记本特有</sup>](#4关闭独显直连笔记本特有)
3. `Configuration` → `SATA Controller Mode`：设置为 `AHCI`，不要`Intel RST Premlun`。（此为Intel的CPU，没有此项就不用管）
4. `Boot` 或者 `Startup`-`Boot` → 将U盘引导项放到最前面，如下图所示。
5. 保存并退出。

![BOOT-U](https://files.seeusercontent.com/2026/08/18/1kZe/U.png)

## 7.从U盘启动

设置好BIOS之后，一般会自动重启，这时候什么也不要动，等显示出如下页面则说明成功进入了：

当然，由于我们只传入了`ubuntu-22.04.5-desktop-amd64.iso`，所以应该仅显示这一项才对。

![U盘启动成功](https://files.seeusercontent.com/2026/08/18/5Siq/20260818210135291.png)

一路回车键按起来，这里应该是最后一步按回车键的截图：

![最后回车键](https://files.seeusercontent.com/2026/08/18/0bgM/20260818210705934.png)

在这个页面按回车之后，就会出现安装程序了：（准备动手！）

![开始安装](https://files.seeusercontent.com/2026/08/18/gn6X/20260818210739633.png)

## 8.安装Ubuntu

从U盘成功启动后，你会看到一个欢迎界面。

1. `选择语言`：在左侧选择“**中文(简体)**”，然后点击右侧的“**安装Ubuntu**”。

2. `键盘布局`：通常保持默认的“Chinese”即可，点击“继续”。

3. `更新和其他软件`：建议取消勾选“**安装Ubuntu时下载更新**”，勾选“**为图形或Wi-Fi硬件安装第三方软件**”，然后点击“继续”。

4. `安装类型`：这是关键一步，请根据你的需求选择：

  - `清除整个磁盘并安装Ubuntu`：这**会删除硬盘上的所有数据**，将Ubuntu作为电脑唯一系统。

  - `与Windows共存`：如果你想保留原有Windows系统，实现双系统启动，就选择此项。（这里我们不选择这个）

  - `其他选项`：适合高级用户手动分区，就**选择此项**。

### 给Ubuntu匹配磁盘空间

还记得[为Ubuntu整理磁盘空间](#5为ubuntu整理出磁盘空间)吗？这时候就要我们分配硬盘给Ubuntu了。

如下图，209717MB的空闲空间就是我们为Ubuntu整理出来的空间。

![](https://files.seeusercontent.com/2026/08/18/Fdi4/20260818212136542.png)

左键单击选中这个空闲分区，点击左下角的加号：

![](https://files.seeusercontent.com/2026/08/18/nNp6/20260818212325741.png)

挂载点选择`/`挂载点，之后点击`OK`：

![](https://files.seeusercontent.com/2026/08/18/dL1o/20260818212403015.png)

之后需要为启动引导器选择位置，找到`Windows Boot Manager`这一项：

![](https://files.seeusercontent.com/2026/08/18/zjU5/20260818212650843.png)

在下面同样选择这一项：

![](https://files.seeusercontent.com/2026/08/18/1Pkw/20260818212737707.png)

所有做完后，点击“现在安装”即可进行下一步。

5. `设置时区`：在地图上点击中国区域，或输入“Shanghai”，然后点击“继续”。

6. `创建用户账户`：输入你的姓名、电脑名称、用户名和密码，然后点击“继续”。（注意，这里的姓名和电脑名称会以`姓名@电脑名称`形式出现在终端中，不宜过长）

7. `等待安装`：之后系统会自动进行复制文件、配置等操作，耐心等待10-20分钟即可。

8. `重启电脑`：安装完成后，点击“现在重启”。

9. `移除U盘`：电脑重启时，拔掉U盘，否则可能会再次进入安装界面。重启后，你就能用刚才设置的账号密码登录全新的Ubuntu系统了。

## 9.改回Boot引导项

在[操作BIOS](#6操作bios)中，我们将U盘引导项放到了第一个，现在我们需要将`Ubuntu`放到第一个，把`Windows Boot Manager`放到第二个。

![](https://files.seeusercontent.com/2026/08/18/1Lmc/20260818213315466.png)

## 可能用到的链接

- [Windows11安装Ubuntu避坑指南 | 哔哩哔哩](https://www.bilibili.com/video/BV1Cc41127B9?p=1){#anchor-sp}
- [Ubuntu下载 | 华为云](http://repo.huaweicloud.com/ubuntu-releases/){#anchor-ubuntu}
- [Ventoy 镜像下载 | 山东大学镜像站](https://mirrors.sdu.edu.cn/github-release/ventoy_Ventoy){#anchor-ventoy}
- [diskgenius | 硬盘分区工具](https://www.diskgenius.cn/download.php){#anchor-diskgenius}
