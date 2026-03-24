---
title: "写博客之必读内容"

description: "用Hugo写博客，需要留意的点。"
date: "2024-11-14T15:13:23+08:00"

categories: ["建站技术"]
tags: ["hugo", "blog"]
reaction: true
top: false
---

## 引言（2024.11.15 更新）

最近想重新拾起写博客的习惯，但是发现之前的[Hexo 博客 | (Github-Pages)](https://rento666.github.io)有点花里胡哨的，找了半天，发现这个[Hugo | (Github)](https://github.com/gohugoio/hugo)的[Stack 主题 | (Github)](https://github.com/CaiJimmy/hugo-theme-stack)挺符合我审美的，就决定用 Hugo 来写博客。

有以下几点需要注意：

- 我使用的版本是[Hugo-stack 主题-魔改版 | (Github)](https://github.com/Mantyke/Hugo-stack-theme-mod)。
- 由于这个版本(截至目前为止)已经两年没有更新了(上次更新时间为 2022 年)，所以对于最新版本的 Hugo，执行`hugo server`可能会报错，**需要自己复制出错原因，询问 AI**（如 ChatGPT 等）**去解决报错**。

## 使用方式（MarkDown 语法）

### 可收缩代码块

{{% details  "这是标题" %}}

```
// 去掉 % 前面的 \
{{\% details   %}}

内容

{{\% /details  %}}
// 去掉 % 前面的 \
```

{{% /details  %}}

### 使用 i18n 文件中的值

须知：

- 我的`i18n`文件夹在`.\themes\stack\i18n`目录下。

下面我将拿主页的欢迎语为例，讲述一下如何适配多语言：

![欢迎语](https://s2.loli.net/2024/11/15/27fZVBizK6J9nsh.png)

这个欢迎语的`html模板`在`.\layouts\index.html`中，位于`{{ $pag := .Paginate ($filtered) }}`之下。

{{% details  "这是html模板" %}}

```
<!-- 首页欢迎字幅板块 -->
<div class="welcome">
  <p style="font-size: 2rem; text-align: center; font-weight: bold">
    <span class="shake">{{ T "welcome.emoji" }}</span>
    <span class="jump-text1"> {{ T "welcome.msg1" }} </span>
    <span class="jump-text2"> {{ T "welcome.msg2" }} </span>
    <span class="jump-text3" style="color: #e99312"> {{ .Site.Title }} </span>
    <!-- <span class="jump-text3" style="color:#e99312">Ca</span><span class="jump-text4" style="color:#e99312">i</span>
        <span class="jump-text5" style="color:#e99312">Ho</span><span class="jump-text6" style="color:#e99312">ng</span>
        <span class="jump-text7" style="color:#e99312">Tu</span>
        <span class="jump-text8" style="color:#e99312">'s</span>
        <span class="jump-text9" style="color:#e99312">Blog</span> -->
  </p>
  <p style="font-size: 1.5rem; text-align: center; font-weight: bold">
    <span
      >{{ T "welcome.currentText" }}:
      <a href="{{ .Site.BaseURL }}"
        >{{ .Site.BaseURL | strings.TrimPrefix "http://" | strings.TrimPrefix
        "https://" | strings.TrimSuffix "/" }}</a
      ></span
    >&emsp;&emsp;
    <span
      >{{ T "welcome.permanentText" }}:
      <a href="https://cai-hong-tu-blog.pages.dev"
        >cai-hong-tu-blog.pages.dev</a
      ></span
    >
  </p>
</div>
<!-- ------首页欢迎字幅板块------ -->
```

{{% /details  %}}

- `{{ T "welcome.emoji" }}` 都是我在`.\themes\stack\i18n`文件夹下的`xxx.yaml`文件中定义的变量（xxx 代表多语言）.

{{% details  "这是zh-CN.yaml文件中的配置" %}}

```
welcome:
    emoji:
        other: 👋
    msg1:
        other: 欢迎
    msg2:
        other: 来到
    permanentText:
        other: 永久博客链接
    currentText:
        other: 当前博客链接
    top:
        other: 回到顶部

```

{{% /details  %}}

- `{{.Site.BaseURL }}` 是我在`.\config\_default\hugo.yaml`文件中定义的变量。(我使用了多语言配置 config，默认应该是./config.yaml)

{{% details  "hugo.yaml前几行的" %}}

```
baseurl: https://caihongtu.top/
languageCode: zh-cn
theme: stack
title: 彩虹兔の博客
```
{{% /details  %}}

### Markdown文档中插入空格

在 Markdown 文档中，可以直接采用 HTML 标记插入空格（blank space），而且无需任何其他前缀或分隔符。具体如下所示：

- 插入一个空格 (**non-breaking space**)  

  `&nbsp;`    或    `&#160;`     或      `&#xA0;`

- 插入两个空格 (**en space**)  

  `&ensp;`     或    `&#8194;`   或      `&#x2002;`

- 插入四个空格 (**em space**)  

  `&emsp;`    或    `&#8195;`   或      `&#x2003;`

- 插入细空格 (**thin space**)  

  `&thinsp;`  或     `&#8201;`  或      `&#x2009;`

  <font style="background: yellow">**注意：不要漏掉分号。**</font>

## 常识（对我来说需要记在这里）

### HTML 字符集——空格

- 空格：`&nbsp;` 按下`space`键出来的，宽度受字体影响。
- 空格：`&ensp;` 宽度为$\frac{1}{2}$个中文宽度，不受字体影响。
- 空格：`&emsp;` 宽度为 1 个中文宽度，不受字体影响。

### 点击 a 标签打开新窗口

在 a 标签中添加`target="_blank"`属性即可。

```
<a href="你的链接" target="_blank"></a>

```

## 未解决问题（随时更新）

- 以下问题是在`hugo v0.138.0`的基础上出现的（可能是我不会操作/doge）。

{{% details  "如何查看hugo版本？" %}}
在 cmd 中输入`hugo version`

```
输出：hugo v0.138.0-ad82998d54b3f9f8c2741b67356813b55b3134b9+extended+withdeploy windows/amd64 BuildDate=2024-11-06T11:22:34Z VendorInfo=gohugoio
```

{{% /details  %}}

### categories 的多语言适配

我尝试了在`./content/categories`目录下的`test`文件中，使用`_index.md`和`_index.en.md`，结果并不是我预想的那样，在**首页分类**处，只能显示为`test`或者`测试`，并没有多语言适配。

![图示右侧栏不适配多语言](https://s2.loli.net/2024/11/14/wJNV8LWTyKCSsom.png)

{{% details  "起初目录结构" %}}

```
├─content
│  ├─categories
│  │  _index.en.md
│  │  _index.md
│  │
│  └─test
│     │    _index.md
│     └─   _index.en.md
```

{{% /details  %}}

最终只能将`test`、`测试`两个目录分开配置：
{{% details  "最终目录结构" %}}

```
├─content
│  ├─categories
│  │  │  _index.en.md
│  │  │  _index.md
│  │  │
│  │  ├─test
│  │  │      _index.en.md
│  │  │
│  │  └─测试
│  │          _index.md
```

{{% /details  %}}

如图所示：
![目录结构](https://s2.loli.net/2024/11/14/ps4S2Jmn9IbtArC.png)

## 附录

### 用到的第三方

- [picsum |随机图片](https://picsum.photos/)
- [SM.MS | 图床](https://sm.ms/home/)

### 参考

- [Hugo 对 Markdown 支持情况测试 | 博客](https://edward852.github.io/post/markdown支持情况测试/)

- [Stack 主题的自定义 | 博客](https://linsnow.cn/posts/bloglab/hugo-stack/modify-hugo/)

- [Windows 安装 Hugo | 文档](https://gohugo.io/installation/windows/)

### 版权信息

本文原载于 [彩虹兔の博客](https://cai-hong-tu-blog.pages.dev/)，遵循 CC BY-NC-SA 4.0 协议，复制请保留原文出处。

---

## 结尾
