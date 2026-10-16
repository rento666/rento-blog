---
title: '博客更换为Reimu主题'
description: 这里写了该主题的一些注意事项，包括但不限于“内置shortcode”……

date: 2026-03-23T09:12:38-07:00
lastmod: 2026-03-24T10:15:53-07:00
categories: ["建站技术"]
tags: ["hugo", "reimu"]
---

主题更换为Reimu主题后，原来主题的短代码就失效了，所以赶快来记录一下现主题的短代码。

## 短代码

[本主题](https://github.com/D-Sketon/hugo-theme-reimu)内置了一些shortcode，可以在markdown文件中直接使用，效果很好看，这里列举一下，就不用去官网看文档了。

### 文章热力图

> 一个页面只能有一个！

这是效果：
{{< heatMapCard levelStandard="1000,5000,10000" >}}

```markdown
{{</*heatMapCard levelStandard="1000,5000,10000"*/>}}
```
|参数名|是否必填|默认值|说明|
|---|---|---|---|
|levelStandard|选填|1000,5000,10000|按照这个字数来分级|

### 标签轮盘

> 一个页面只能有一个！

这是效果：
{{< tagRoulette tags="打电动,看剧,看小说" icon="🎲" >}}

```markdown
{{</*tagRoulette tags="打电动,看剧,看小说" icon="🎲"*/>}}
```
|参数名|是否必填|默认值|说明|
|---|---|---|---|
|tags|选填|带有模糊效果,标签轮盘,点击按钮抽取标签|指定标签池|
|icon|选填|🕹️|按钮图标|

### 块引用

这是效果：
{{< alertBlockquote type="note" >}}
Your content here
{{</alertBlockquote>}}

```markdown
{{</*alertBlockquote type="note"*/>}}
Your content here
{{</*/alertBlockquote*/>}}
```

|参数名|是否必填|默认值|说明
|---|---|---|---|
|type|选填|note|其他可选有：tip、important、warning、danger|

### 链接卡片
这是效果：
{{< link title="短代码放到代码块中转义问题" link="https://github.com/D-Sketon/hugo-theme-reimu/issues/108" cover="https://github.githubassets.com/assets/apple-touch-icon-144x144-b882e354c005.png" escape="true" >}}

{{< link link="/post/2026/reimu-theme/" cover="auto" >}}
```markdown
{{</*link link="/post/2026/reimu-theme/" cover="auto"*/>}}
```

|参数名|是否必填|默认值|说明|
|---|---|---|---|
|title|必填|-|link/path为内链时，可省略，自动使用文章标题
|link/path|必填|-|URL地址，link、path均可使用，二者效果相同
|cover|选填|auto| 卡片展示的封面，如果设置为 auto 则自动使用博客的 banner 或缺省封面
|escape|选填|true|文章标题是否被转义，可选true或false

### 标签页
这是效果：
{{< tabs 2 "center" >}}
<!-- tab 标签页1名字 -->
Tab1 content
123123
<!-- tab标签页2名字@e60c -->
Tab2 content
6666666
{{< /tabs >}}

```markdown
{{</*tabs 2 "center"*/>}}
<!-- tab 标签页1名字 -->
Tab1 content
123123
<!-- tab标签页2名字@e60c -->
Tab2 content
6666666
{{</*/tabs*/>}}
```

|参数名|是否必填|默认值|说明|
|---|---|---|---|
|activeTab|选填|-|指定默认激活的标签页下标，从 1 开始计数，不写默认为1|
|"center"|选填|-|指定标签页标题居中显示，不写默认左对齐|
|tabName|必填|-|每个标签页的标题，必须用 `<!-- tabName -->` 包裹，支持使用 @ + 图标十六进制代码展示图标 |

### 照片墙
这是效果：
{{< gallery >}}
![image](https://picsum.photos/800/600)
![image](https://picsum.photos/600/800)
![image](https://picsum.photos/700/500)

{{< /gallery >}}

```markdown
{{</*gallery*/>}}
![image](https://picsum.photos/800/600)
![image](https://picsum.photos/600/800)
![image](https://picsum.photos/700/500)
{{</*/gallery*/>}}

```

### 网格布局
```markdown
{{</* grid width=? col=? */>}}
<!-- cell -->
内容1
<!-- cell -->
内容2
<!-- cell -->
内容3
{{</* /grid */>}}
```
|参数名|是否必填|默认值|说明|
|---|---|---|---|
|width|选填|240|设置最小列宽，如 300 表示最小列宽为 300px|
|col|选填|-|设置固定列数，如 3 表示固定 3 列布局，默认值为自适应列数|

使用 `<!-- cell -->` 分隔每个网格单元，每个单元的内容会被独立渲染

### 折叠面板
这是效果：
{{< details summary="?" >}}
内容
{{< /details >}}
```markdown
{{</* details summary="?" */>}}
内容
{{</* /details */>}}
```

|参数名|是否必填|默认值|说明|
|---|---|---|---|
|summary|选填|-|标题，默认为空|