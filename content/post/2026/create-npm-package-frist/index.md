---
title: '创建第一个npm包'
description: 这里介绍了创建并上传npm包(ai-zhipu-free-sdk)的过程。   

date: 2026-03-28T23:10:04-07:00
lastmod: 2026-03-29T15:13:17-07:00
categories: ["学习笔记"]
tags: ["ai", "npm", "package"]
---

## 介绍

什么是npm包？

**npm 包**（Node Package Manager Package），是**Node.js 生态系统中，用于封装、复用和分发 JavaScript 代码的标准化模块单元**。简单说，它就是一个打包好的代码工具库，可以被直接下载、引用到项目中使用。

在本篇文章中，我们创建了一个npm包——ai-zhipu-free-sdk，主要目的就是封装智谱免费模型的API调用，方便我们其他项目快捷、方便地使用！

## 准备

我们首先就要将**可复用的代码整理到一个项目中，并提供友好的对外调用接口**。

在[ai-zhipu-free-sdk](https://www.npmjs.com/package/ai-zhipu-free-sdk)项目中，我们封装了智谱免费模型的API调用，提供了简单的调用接口。

### 项目结构

为了项目的可维护性和可扩展性，一般的，npm包的项目结构如下：

```
ai-zhipu-free-sdk
├── src
│   ├── index.ts
├── .env
├── package.json
├── tsconfig.json
├── README.md
├── LICENSE
```

从package.json可以知道**入口文件**的名称是index.ts，所以你只需要看index.ts文件即可。

index.ts主要用来export项目中的函数、类等，方便其他开发者调用。

所以如果你想知道一个包可以使用哪些方法，一般只需要看index.ts文件即可。

## 发布

当项目开发完成后，我们就要发布到npm仓库中了。

- 首先发布到GitHub，这个不教了，应该都清楚。
- 登录npm仓库 [npm](https://www.npmjs.com/)
    - 除了在官网登录之外，还需要在cmd中登录`npm login`。
    - 登录后，你可以在cmd中使用`npm publish`命令发布到npm仓库中。
- 启用2FA，目前（2026-03-29）不启用这个，发布时会报错。
- 使用 changesets 进行版本管理，每次发布时都要更新版本号。
    - 先安装`npm i -g changesets`。
    - 然后执行`npm run changesets`命令，创建变更记录。
    - 更新版本号`npm run version`。
    - 最后发布到npm仓库中`npm run release`。

{{<alertBlockquote type="note">}}
请注意，上面所使用的`npm run xxx`，均为`package.json`中定义的脚本。

```json
{
    "scripts": {
        "changeset": "changeset",
        "version": "changeset version",
        "release": "npm run build && changeset publish"
    }
}
```
{{</alertBlockquote>}}


## 感谢

- [ai-zhipu-free-sdk | npm](https://www.npmjs.com/package/ai-zhipu-free-sdk)
- [智谱AI开放平台 | BigModel](https://open.bigmodel.cn/)
- [智谱AI API文档 | BigModel](https://docs.bigmodel.cn/)