---

title: "个人推荐的一些应用、APP、网站"
slug: "apps"
description: "内含你想要的工具！"
date: "2024-12-08T21:44:34+08:00"
lastmod: "2025-08-07T11:18:21+08:00"
categories: ["软件安利"]
tags: ["app", "software"]

weight: -99
---

## 总览

| 软件分类      | 描述 |
| :-: | :-: |
| [娱乐类](#娱乐类) | 游戏、音乐等软件、工具 |
| [工具类](#工具类) | 我用到的一些小工具 |

## 娱乐类

### MusicFree-音乐软件（支持多端）

[MusicFree](https://musicfree.catcat.work/)一款免费、开源、可以上传音乐源（**插件化**）的音乐播放器，支持Mac、Linux、Windows、Android四端（截止2025.08.07）。

也可以从[MusicFreeDesktop Github](https://github.com/maotoumao/MusicFreeDesktop)的**Releases**下载。

下载 app 之后，只需要在侧边栏设置-插件设置中安装插件即可。你可以直接点击从网络安装插件，然后输入:
```
https://gitee.com/maotoumao/MusicFreePlugins/raw/master/plugins.json
```
点击确认即可安装。

然后你就可以进行搜索音乐啦~

> 个人建议，选择**bilibili**或者**YouTube**音乐源更好，这两个的音乐也更全。

### MoonTV-视频网站（可自行私密部署）

[MoonTV](https://github.com/LunaTechLab/MoonTV)是一个开箱即用的、跨平台的影视聚合播放器。

内置数十个**免费**资源站点，一次搜索立刻返回全源结果（实测部分站点的速度很快）。

电影、电视剧、综艺、纪录片应有尽有！

可以部署到[Vercel](https://vercel.com/)，实现私人影院！

1. **Fork**[本仓库](https://github.com/LunaTechLab/MoonTV)到你的 GitHub 账户。
2. 登陆 **Vercel**，点击 **Add New → Project**，选择 Fork 后的仓库。
3. 设置 **PASSWORD** 环境变量（访问密码）。
4. 保持默认设置完成首次部署。
5. 如需自定义 `config.json`，请直接修改 Fork 后仓库中该文件。
6. 每次 **Push** 到 `main` 分支将自动触发重新构建。

部署完成后即可通过分配的域名访问，也可以绑定自定义域名。

> 个人建议绑定一个自定义域名，这样国内也能顺利打开网站（毕竟Vercel的域名被国内给墙了）

## 工具类

### Watt Toolkit (原名steam++)

[Watt Toolkit](https://steampp.net/)是一个开源跨平台的多功能 Steam 工具箱。

之所以将其划分到[工具类](#工具类), 是因为我觉得这个除了加速[steam商店](https://store.steampowered.com/), 还能加速其他的网站，包括但不限于[Discord语音聊天 | 3.0.0-rc.13(2024-12-01)版本疑似已去除](https://discord.com/)、[Twitch直播](https://www.twitch.tv/)、[Origin游戏下载 - 烂橘子](https://www.origin.com/)、[Uplay商店 - 育碧](https://zh-cn.ubisoft.com/ubisoftconnect)、[一些公共CDN](#参考)、[Google翻译](https://translate.google.com/)、[Spotify](https://www.spotify.com)、[一些国外验证码平台](#参考)、[Github](https://www.github.com)、[Roblox](https://www.roblox.com)、[Mod.io](https://www.mod.io)、[Nexus Mods - N网MOD](https://www.nexusmods.com)、[一些网盘服务](#参考)、[其他网站](#参考)

### 魔戒（薇屁恩）- 按流量计费、无限时间

[魔戒 VPN注册](https://mojie.cyou/#/register?code=PuxpBY7b)是我见过的唯一一个按流量计费、不限时间的节点订阅网站。

通过我这个链接`https://mojie.cyou/#/register?code=PuxpBY7b`注册账号，首次购买套餐有20%折扣！

> 就我个人而言，130GB流断断续续用了3、4年还有15%左右！

![套餐详情](https://s2.loli.net/2025/08/15/QzwZMekXqBW5YFu.png)

### SubsTracker-订阅管理与提醒系统

[SubsTracker](https://github.com/wangwangit/SubsTracker)是一款基于Cloudflare Workers的轻量级订阅管理系统。

支持多渠道通知：Telegram Bot 通知、NotifyX推送服务、企业微信应用通知、企业微信机器人、基于 Resend 的邮件服务以及自定义请求格式和模板。

部署到CloudFlare Workers 应该不必多教，项目的README中也有教程。还有不会的可以在评论区留言。

> 为什么推荐这个项目？因为我用了一个免费的域名，需要在到期前180天内续费一次，所以用这个设置一下提醒，怕忘记了。<br>
> 这个项目的用处当然不止于此，或许有其他用法尚未挖掘。

### 免费域名(xxx.dpdns.org)

[Digitalplat](https://digitalplat.org/)点击`Projects`,然后选择`Free domain for everyone`，需要注册一个账号。

![免费域名](https://s2.loli.net/2025/08/07/VqSR29vOgHMrfnu.png)

注册好账号后点这个，注册域名：

![进入注册域名页面](https://s2.loli.net/2025/08/07/TyjeXwE6o9Ztlmg.png)

然后输入你想要的域名前缀，检查能不能用，如果能用，则直接就弄好了，如果不行，需要更改前缀名。

![最后点击Check即可](https://s2.loli.net/2025/08/07/C9qwbv2KWZiog3D.png)

创建好后需要搞上DNS服务器，推荐CloudFlare，只需要在上面添加该域名，然后就会有提示填写两个DNS服务器。

填写后需要保存，成功截图如下：

![免费域名绑定CloudFlare的DNS服务器](https://s2.loli.net/2025/08/07/R2JjtHZGe7kbdDV.png)

（不会域名绑定到Cloudflare的可以评论区留言）

> 应该可以有多个二级域名,例如我的域名是`caihongtu.dpdns.org`,那么可以有二级域名: `a.caihongtu.dpdns.org`、`b.caihongtu.dpdns.org`等等.

后续的续费操作如下图所示：

![续费免费域名](https://s2.loli.net/2025/08/07/UKrpyOXgcln95fQ.png)

## 附录

以下根据watt toolkit 的**3.0.0-rc.13(2024-12-01)版本**介绍。

- 公共CDN有[fonts.gstatic.com](fonts.gstatic.com)、[Gravatar头像](gravatar.com)、[themes.googleusercontent.com](themes.googleusercontent.com)、[ajax.googleapis.com](ajax.googleapis.com)、[fonts.googleapis.com](fonts.googleapis.com)、[BootStrap CDN](maxcdn.bootstrapcdn.com/bootstrap)

- 国外验证码平台有[Goole(Recaptcha)验证码](https://www.google.com/recaptcha)、[hCaptcha验证码](https://www.hcaptcha.com)、[Arkoselabs](https://www.arkoselabs.com)

- 网盘服务有[OneDrive](https://onedrive.live.com)、[MEGA](https://mega.io)、[DropBox](https://dropbox.com)

- 其他网站有[Pinterest](https://www.pinterest.com)、[Artstation](https://www.artstation.com)、[Imgur图床](https://www.imgur.com)、[vercel.app](https://vercel.app)、[appcenter.ms](https://appcenter.ms)

### 参考

- [VS Code 的 7 个开源替代品 | Linux中国](https://linux.cn/article-12382-1.html)

### 版权信息

本文原载于 [彩虹兔の博客](https://cai-hong-tu-blog.pages.dev/)，遵循 CC BY-NC-SA 4.0 协议，复制请保留原文出处。
