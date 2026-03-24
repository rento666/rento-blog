---

title: "Outlook邮箱的Smtp服务出现Oauth2认证失败错误"
description: "在本博客的评论系统接入邮箱提醒时，用到了outlook的smtp服务，但是在使用时却发现了一个错误。"
date: "2024-11-15T22:57:11+08:00"
lastmod: "2024-11-15T22:57:11+08:00"

categories: ["踩坑记录"]
tags: ["outlook", "smtp"]

---

## 引言

在本博客的评论系统接入邮箱提醒时，用到了outlook的smtp服务，但是在使用时却发现没有邮件提醒。

## 查看日志

于是上vercel看了一下日志，如下图：

![vercel部署的waline项目——发送邮件提醒报错](https://s2.loli.net/2024/11/15/wl5o4EAj2GBv3sI.png)

报错堆栈如下：
```
Mail send fail: Error: Invalid login: 535 5.7.139 Authentication unsuccessful, basic authentication is disabled. [CH0PR13CA0024.namprd13.prod.outlook.com 2024-11-15T12:41:27.364Z 08DD02E8CBA00082]
    at SMTPConnection._formatError (/var/task/node_modules/nodemailer/lib/smtp-connection/index.js:807:19)
    at SMTPConnection._actionAUTHComplete (/var/task/node_modules/nodemailer/lib/smtp-connection/index.js:1586:34)
    at SMTPConnection.<anonymous> (/var/task/node_modules/nodemailer/lib/smtp-connection/index.js:1540:18)
    at SMTPConnection._processResponse (/var/task/node_modules/nodemailer/lib/smtp-connection/index.js:991:20)
    at SMTPConnection._onData (/var/task/node_modules/nodemailer/lib/smtp-connection/index.js:772:14)
    at TLSSocket.SMTPConnection._onSocketData (/var/task/node_modules/nodemailer/lib/smtp-connection/index.js:195:44)
    at TLSSocket.emit (node:events:519:28)
    at TLSSocket.emit (node:domain:488:12)
    at addChunk (node:internal/streams/readable:559:12)
    at readableAddChunkPushByteMode (node:internal/streams/readable:510:3) {
  code: 'EAUTH',
  response: '535 5.7.139 Authentication unsuccessful, basic authentication is disabled. [CH0PR13CA0024.namprd13.prod.outlook.com 2024-11-15T12:41:27.364Z 08DD02E8CBA00082]',
  responseCode: 535,
  command: 'AUTH LOGIN'
}
```

## 此文章来源

在[waline的discussions](https://github.com/orgs/walinejs/discussions)貌似没人提出这个问题，可能是只有我在用Outlook吧...  


看第一行就行：`Mail send fail: Error: Invalid login: 535 5.7.139 Authentication unsuccessful, basic authentication is disabled.`，意思是说outlook的smtp服务不支持basic authentication。  
我搜了一下，发现Outlook在24年9月16日停止了个人用户对基础身份验证的支持，推荐使用OAuth2认证。[详见 | Microsoft 官网](https://techcommunity.microsoft.com/blog/outlook/keeping-our-outlook-personal-email-users-safe-reinforcing-our-commitment-to-secu/4164184)   
但是我没在[waline文档](https://waline.js.org/reference/server/env.html#邮件)里发现支持OAuth2。  

自己写插件吗？

似乎没有必要为了一个评论系统写，毕竟人的精力是有限的（~~我才不会说是我菜~~）

## 解决方案

于是就有了今天的主角：[sendas.email](https://sendas.email/)   

我这里就用作者部署好的项目，就不再重新部署了。
如果你想自己部署，可以先fork一下作者的项目，然后自己部署。
部署方法如下图：

![部署方法](https://s2.loli.net/2024/11/16/Z2ROoCiN78X9bcA.jpg)

### 开始解决

进入[sendas.email](https://sendas.email/)，然后滚轮滑倒最下方，点击`Sign in with Microsoft`  

![Sign in with Microsoft | sendas.email](https://s2.loli.net/2024/11/16/nvXBIkVYOZ2CSQx.png)

登录时，Microsoft会提示你是否允许第三方应用访问你的账号，点击**接受**即可。

![是否授权](https://s2.loli.net/2024/11/16/5OY3qFZXu2NGvgb.png)

授权之后Microsoft账户团队就会发一个邮件提醒：

![邮件提醒](https://s2.loli.net/2024/11/16/SkPVCUp45gBunwI.png)

此时回到sendas.email,网站自动重定向到了[configuration页面](https://sendas.email/configuration),页面如下图所示：  

![显示新的用户名+密码](https://s2.loli.net/2024/11/16/XV5Gr8wY1hdtJ4H.png)

### 修改环境变量

下面回到vercel的部署，将之前的密码换成新的密码即可。

![对应关系](https://s2.loli.net/2024/11/16/ixrA7yskvpw9H4h.png)

请注意，如果你之前使用的是`SMTP_SERVICE`，那么现在需要删除掉这个，然后新增`SMTP_HOST`和`SMTP_PORT`.

![使用的是SMTP_SERVICE](https://s2.loli.net/2024/11/16/zxqafYFO6iVjIMR.png)

完成vercel的环境变量配置，如下图：

![邮箱配置](https://s2.loli.net/2024/11/16/aHElFG3j1M6XZwC.png)

重启项目，然后就可以正常发送邮件了。

![重启项目](https://s2.loli.net/2024/11/16/vncROktPExoCH8h.png)


## 附录

### 参考

* [waline discussions | Github](https://github.com/orgs/walinejs/discussions)  
* [sendas.email | 这次的主角(this target)](https://sendas.email/)  
* [Gmail-to-outlook-proxy | Github](https://github.com/jasperchan/gmail-to-outlook-proxy)  
* [Gmail Help | 谷歌邮箱 Community](https://support.google.com/mail/thread/298145809/outlook-smtp-authentication-errors?hl=en)  
* [更新政策 | Microsoft 官网](https://techcommunity.microsoft.com/blog/outlook/keeping-our-outlook-personal-email-users-safe-reinforcing-our-commitment-to-secu/4164184)  
* [错误的微软凭据 | Microsoft](https://support.microsoft.com/en-us/office/pop-imap-and-smtp-settings-for-outlook-com-d088b986-291d-42b8-9564-9c414e2aa040)

### 版权信息

本文原载于 [彩虹兔の博客](https://cai-hong-tu-blog.pages.dev/)，遵循 CC BY-NC-SA 4.0 协议，复制请保留原文出处。