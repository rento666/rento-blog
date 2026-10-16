---

title: "Taro 使用 Tailwindcss，解决init报错"
slug: "Taro Use Tailwindcss"
description: "国内的tailwindcss版本较低"
date: "2025-07-16T12:06:06+08:00"
lastmod: "2025-07-16T12:06:06+08:00"
image: ""
math: 
license: 
hidden: false
draft: false 
categories: ["踩坑记录"]
tags: ["taro", "tailwindcss"]
reaction: true
top: false

---

> Taro框架官网对于引入Tailwindcss写的并不准确<sup>[[1]](#参考)</sup>，因为在进行`init`的时候会报错。

## 起因

使用官网的方法安装：

```
npm i -D tailwindcss postcss autoprefixer
# 初始化 tailwind.config.js 文件
npx tailwindcss init
```

在进行第二步的时候，会出现报错：
```

npm ERR! could not determine executable to run
`npm ERR!
A complete log of this run can be found in: C:\Users\Administrator\AppData\Local\npm-cache_logs\2025-07-16T07_09_25_220Z-debug-0.log
```

## 分析

打开项目的`node_modules`，找到`tailwindcss`，可以发现它的版本是`4.1.11`，而打开[tailwindcss官网](https://www.tailwindcss.cn/docs/installation)，可以看到左上角的版本为`3.4.17`

> 仅限国内，[国际版](https://tailwindcss.com/)貌似支持了`4.1`版本。

所以只需要将这个版本降下来就行。

## 解决办法

依次在cmd中运行如下：

```
npm uninstall tailwindcss

npm install -D tailwindcss@3.4.17 postcss autoprefixer

npx tailwindcss init
```

后边的跟着[官网](https://nervjs.github.io/taro-docs/docs/tailwindcss)走就行了。

## 附录

### 参考

- [使用 Tailwind CSS | Taro官网](https://nervjs.github.io/taro-docs/docs/tailwindcss)


