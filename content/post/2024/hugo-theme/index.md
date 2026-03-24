---

title: "关于博客的样式改造"
description: "讲解了首页的欢迎横幅、置顶文章……"
date: "2024-11-21T12:08:45+08:00"
lastmod: "2024-11-21T12:08:45+08:00"

categories: ["建站技术"]
tags: ["hugo", "blog"]

---

## 引言

- 本博客使用了[stack主题 | hugo](https://github.com/rento666/My-Blog-By-Hugo-stack)，原GitHub仓库为[Hugo-stack-theme-mod | GitHub](https://github.com/Mantyke/Hugo-stack-theme-mod)

- 如果你的**Hugo版本**是高于**2022年**发布的，直接运行源仓库代码会报错，因为其中部分函数方法已经弃用，修改请参考：将报错原因发送给AI，根据AI提示错误来修改，这里由于我修改时没截图，暂时无法展示。

- 如果使用[我的博客仓库代码](https://github.com/rento666/My-Blog-By-Hugo-stack)，请将我的文章全部删除后，再添加你自己的文章。同时需要替换所有出现的`彩虹兔`、`caihongtu`之类的词语。

## 首页的欢迎横幅

> 2025年10月14日更新，域名已经更改为`caihongtu.top`

预览：
![欢迎横幅](https://s2.loli.net/2024/11/21/XhInQYoesvJf1N3.png)

由于项目是部署在Cloudflare上的，所以那个**永久域名**大概不会过期（~~除非Cloudflare倒闭~~），但是**当前域名**可就不一定了，因为我想要注册的**caihongtu.com**目前被人注册了，所以先注册了这个`caihongtu.asia`，等那个`com`域名可以注册了，我会立马出手的。（别问为什么不去找对方买，因为没💰）

### 修改代码

- 博客的根目录新建`layouts`文件夹（默认就有，没有的话手动创建），之后将`./themes/hugo-theme-stack/layouts/index.html`下的文件复制到刚刚创建的layouts文件夹内（如果这个文件夹下本来就有对应文件，则无须复制），之后再在`index.html`中添加以下内容：

{{% details  "点我查看代码" %}}
```html
{{ define "main" }} {{ $pages := where .Site.RegularPages "Type" "in"
.Site.Params.mainSections }} {{ $notHidden := where .Site.RegularPages
"Params.hidden" "!=" true }} {{ $filtered := ($pages | intersect $notHidden) }}
{{ $pag := .Paginate ($filtered) }}

<!-- 首页欢迎字幅板块 -->
<div class="welcome">
  <p style="font-size: 2rem; text-align: center; font-weight: bold">
    <span class="shake">{{ T "welcome.emoji" }}</span>
    <span class="jump-text1"> {{ T "welcome.msg1" }} </span>
    <span class="jump-text2"> {{ T "welcome.msg2" }} </span>
    <span id="site-title-static" style="color: #e99312">{{ .Site.Title }}</span>
    <span id="site-title" style="display: none; color: #e99312"></span>
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
<script type="module">
  // 动态生成字符和样式
  document.addEventListener("DOMContentLoaded", () => {
    const title = "{{ .Site.Title }}"; // 获取 Hugo 变量
    const titleContainer = document.getElementById("site-title");
    const staticTitle = document.getElementById("site-title-static");

    // 动态添加字符到 #site-title
    title.split("").forEach((char, index) => {
      const span = document.createElement("span");
      span.textContent = char;
      span.className = `jump-text jump-text${index}`;
      titleContainer.appendChild(span);
    });

    // 动态生成 CSS 动画规则
    const styleSheet = document.styleSheets[0];
    title.split("").forEach((_, index) => {
      const delay = (index * 0.1).toFixed(1); // 每个字符延迟 0.1s
      const rule = `
        .jump-text${index} {
          display: inline-block;
          animation: jump 0.5s 1;
          animation-delay: ${delay}s;
        }
      `;
      styleSheet.insertRule(rule, styleSheet.cssRules.length);
      staticTitle.style.display = "none";
      titleContainer.style.display = "inline";
    });
  });
</script>
<!-- ------首页欢迎字幅板块------ -->

<section class="article-list">
  {{ range $index, $element := $pag.Pages }} {{ partial "article-list/default" .
  }} {{ end }}
</section>
<script type="module">
  import { commentCount } from "https://unpkg.com/@waline/client@v3/dist/comment.js";

  commentCount({
    serverURL: "{{.Site.Params.comments.waline.serverURL}}",
  });
</script>

{{- partial "pagination.html" . -}} {{- partial "footer/footer" . -}} {{ end }}
{{ define "right-sidebar" }} {{ partial "sidebar/right.html" . }} {{ end }}

```
{{% /details  %}}
 
- 上面是我的`index.html`所有的代码，你只需要关注“首页欢迎字幅板块”即可。

下面是横幅的css样式，目录在`./assets/scss/custom.scss`中
{{% details  "点我查看代码" %}}
```
//---------------------------------------------------------
//首页欢迎板块样式
.welcome {
  color: var(--card-text-color-main);
  background: var(--card-background);
  box-shadow: var(--shadow-l2);
  border-radius: 30px;
  display: inline-block;
}

// 👋emoji实现摆动效果
.shake {
  display: inline-block;
  animation: shake 1s;
  animation-duration: 1s;
  animation-timing-function: ease;
  animation-delay: 0s;
  animation-iteration-count: 1;
  animation-direction: normal;
  animation-fill-mode: none;
  animation-play-state: running;
  animation-name: shake;
  animation-timeline: auto;
  animation-range-start: normal;
  animation-range-end: normal;
  animation-delay: 2s;
  @keyframes shake {
    0% {
      transform: rotate(0);
    }
    25% {
      transform: rotate(45deg) scale(1.2);
    }
    50% {
      transform: rotate(0) scale(1.2);
    }
    75% {
      transform: rotate(45deg) scale(1.2);
    }
    100% {
      transform: rotate(0);
    }
  }
}
// 实现字符跳动动画
.jump-text1 {
  display: inline-block;
  animation: jump 0.5s 1;
}

.jump-text2 {
  display: inline-block;
  animation: jump 0.5s 1;
  animation-delay: 0.1s;
}

@keyframes jump {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
  100% {
    transform: translateY(0);
  }
}
```
{{% /details  %}}


## 文章置顶功能（支持多个置顶）

预览：
![置顶文章](https://s2.loli.net/2025/08/16/PTnSfqptNZCXQuz.png)

有时候不得不承认，早期写的文章还是会经常拿出来看的，所以便打算自己写一个置顶功能。

一开始预想的是在文章的头部`Front Matter`写上一个自定义的`top`，为`true`时则置顶，反之则不置顶。

修改代码过程中发现，可以在每篇文章处写上weight。

两者各有利弊，下面分享一下两种置顶的方法。在最后，我将两者稍微整合了一下。

> 温馨提示：如果你赶时间，请直接看最终版！！！

### 使用自定义top属性

在文章中添加这个top：
![添加了top属性](https://s2.loli.net/2025/08/16/GR5ZXMHqB7ylvbV.png)

首先需要搞明白的是，我们想达成的效果：`在首页，文章列表需要将当前页的置顶文章放到最前面，并且有角标标注。`

所以我们可以将其拆分为两件事去做。

#### 1.置顶文章渲染

已知`layouts/index.html`为首页的html模板，所以我们只需要修改这里面的代码即可。

如果你熟悉go中html的模板，那么你一定能看得懂下面代码。

不懂也没关系，简单来说，现在你已经有了置顶文章`topPages`和正常文章`normalPages`！


{{% details  "点我查看代码" %}}
```html
<!-- 修改前 -->
{{ define "main" }} 
{{ $pages := where .Site.RegularPages "Type" "in" .Site.Params.mainSections }} 
{{ $notHidden := where .Site.RegularPages "Params.hidden" "!=" true }} 
{{ $filtered := ($pages | intersect $notHidden) }}
{{ $pag := .Paginate ($filtered) }}

<!-- 修改后 -->
{{ define "main" }} 
{{ $pages := where .Site.RegularPages "Type" "in" .Site.Params.mainSections }} 
{{ $topPages := where $pages "Params.top" true }}
{{ $normalPages := where $pages "Params.top" false }}
{{ $pag := .Paginate ($normalPages) }}
```
{{% /details  %}}

> 小声逼逼，这个`pag`经过`.Paginate`分页之后，就变回了正常的分页，也就是说，`pag`中包含着置顶文章！而这个在我们后面是明显不需要的！<br>
> 并且，我也尝试过将`topPages`和`normalPages`合并后，再进行`.Paginate`分页，但最终结果与其一致！

反正是一顿操作过后，下面的渲染写成了这样：

{{% details  "点我查看代码" %}}
```html
<!-- 这个是原来的 -->
<section class="article-list">
  {{ range $index, $element := $pag.Pages }} {{ partial "article-list/default" .
  }} {{ end }}
</section>

<!-- 分割线 -->

<!-- 这个是我改了之后的 -->
<section class="article-list">
  {{ range $index, $element := $topPages }} {{ partial "article-list/default" .
  }} {{ end }}
</section>
<section class="article-list">
  {{ range $index, $element := $pag.Pages }} 
    {{ if not .Params.top }}
      {{ partial "article-list/default" . }}
    {{ end }}
  {{ end }}
</section>
```
{{% /details  %}}

#### 2.置顶角标

> Q: 如何寻找主页列表是哪个html渲染的？<br>
> A: 我们可以在`index.html`中看到这段代码：`{{ partial "article-list/default" . }}`，这个就说明需要寻找`article-list/default.html`

所以，`layouts\partials\article-list\default.html`为主页的渲染列表，然后打开会发现，它其实是渲染了文章的`header`部分(`layouts\partials\article\components\header.html`)。

- 需要注意，在`header.html`中，如果使用`.IsHome`来判断是否处于首页，其结果永远为`false`，因为这个`.`没有代表着根页面，它是文章页面的一部分！

那么无法判断是否在首页，这个咋办？传递一个参数？不行！这样需要修改的页面就多了，其他用到header的都要加上这个参数。

于是我苦思冥想，终于找到了一种办法，那就是**COPY一份header.html，重命名为header-top.html**

然后让`article-list/default.html`来渲染`header-top.html`就行了！

最终`header-top.html`代码如下：

{{% details  "点我查看代码" %}}
```html
<header class="article-header">
    {{- $image := partialCached "helper/image" (dict "Context" . "Type" "article") .RelPermalink "article" -}}
      {{ if $image.exists }}

        <!-- 新增代码如下 -->
        <div class="article-image">
          {{ if .Params.top }}
            <span class="top-badge">置顶</span>
          {{ end }}
        </div>
        <!-- 新增代码如上 -->

        <!-- 以下代码未动 -->
        <a>...</a>
        <!-- 以上代码未动 -->
    {{ end }}
    {{ partialCached "article/components/details" . .RelPermalink }}
</header>
```
{{% /details  %}}

CSS代码就需要写到`assets\scss\partials\article.scss`中，打开该文件，找到如下图所示的`.article-list article .article-image`,然后填入代码。

![CSS填入位置](https://s2.loli.net/2025/08/16/YmZogktbLzCGAKv.png)

{{% details  "点我查看代码" %}}
```scss
position: relative;
.top-badge {
    position: absolute;
    left: 10;
    top: 10;
    background: #e43;
    color: #fff;
    padding: 6px 30px;
    font-size: 1.5rem;
    font-weight: 700;
    transform: rotate(-35deg)translate(-20px,0px);
    box-shadow: 0 2px 8px rgba(0,0,0,.15);
    z-index: 2;
    border-top-left-radius: 6px;
    border-bottom-right-radius: 6px;
    pointer-events: none;
    user-select: none;
}
```
{{% /details  %}}

到此，大功告成！

#### 缺点

但是这样写有个坏处，就是置顶的没有参与分页。

打个比方说，现在我们在`config`中设置的分页数量为`5`，一共`10`篇文章，其中`后3篇`全需要置顶。

那么第一页是正常显示5篇文章，第二页则是会显示`3+5=8篇`文章！

### 使用weight属性

如果希望置顶的文章也参与分页，那么可以看一看这种方法。

> Hugo中默认就有weight属性，并且分页时会自动计算，所以无需其他多余操作。

只需要在文章的`Front Matter`中添加上`weight: 非零整数`即可！

![image.png](https://s2.loli.net/2025/08/16/fpg1QSkAZRi5JlM.png)

- 请注意，若不写`weight`，则默认视为`0`，并且数字越大，文章越靠后。

- 所以，数字越小（负整数），排名越靠前，也就是等效于“置顶”效果了。

而此时的`header-top.html`，则需要改为如下代码：

```html
{{ if lt .Params.weight -10 }}
    <span class="top-badge">置顶</span>
{{ end }}
```

上面代码是判断`weight`小于`-10`时，才会有置顶角标。

#### 优缺点

使用weight的优点就是`置顶文章也参与到分页中了，进入第二页，则第一页的置顶文章消失`

但缺点也显而易见，需要记住自己填写的这个判断范围，如果超出了这个范围，可能角标不会生效。

那么就会有人问了，煮波煮波，有没有既让置顶文章加入分页，又不用记这个范围的方法呢？我的回答是，有的，兄弟，有的。

### top + weight 组合（最终版）

利用weight来对文章列表渲染进行排序，利用top来控制角标的显隐，应该是最优解了。

![既有top，又有weight](https://s2.loli.net/2025/08/16/XDAe54vFuwKSYyz.png)

`layouts\index.html`保持原代码不变，复制`layouts\partials\article\components\header.html`一份，修改名称为`header-top.html`，路径如下:`layouts\partials\article\components\header-top.html`

`header-top.html`代码如下：
```html
<header class="article-header">
    {{- $image := partialCached "helper/image" (dict "Context" . "Type" "article") .RelPermalink "article" -}}
    {{ if $image.exists }}
        <div class="article-image">
            {{ if .Params.top }}
                <span class="top-badge">置顶</span>
            {{ end }}
            <a href="{{ .RelPermalink }}">
                {{ if $image.resource }}
                    {{- $Permalink := $image.resource.RelPermalink -}}
                    {{- $Width := $image.resource.Width -}}
                    {{- $Height := $image.resource.Height -}}
                    {{- $Srcset := "" -}}
                    
                    {{- if (default true .Page.Site.Params.imageProcessing.cover.enabled) -}}
                        {{- $thumbnail := $image.resource.Resize "800x" -}}
                        {{- $thumbnailRetina := $image.resource.Resize "1600x" -}}
                        {{- $Srcset = printf "%s 800w, %s 1600w" $thumbnail.RelPermalink $thumbnailRetina.RelPermalink -}}
                        {{- $Permalink = $thumbnail.RelPermalink -}}
                        {{- $Width = $thumbnail.Width -}}
                        {{- $Height = $thumbnail.Height -}}
                    {{- end -}}

                    <img src="{{ $Permalink }}"
                        {{ with $Srcset }}srcset="{{ . }}"{{ end }}
                        width="{{ $Width }}" 
                        height="{{ $Height }}" 
                        loading="lazy"
                        alt="Featured image of post {{ .Title }}" />
                {{ else }}
                    <img src="{{ $image.permalink }}" loading="lazy" alt="Featured image of post {{ .Title }}" />
                {{ end }}
            </a>
        </div>
    {{ end }}

    {{ partialCached "article/components/details" . .RelPermalink }}
</header>
```

修改`layouts\partials\article-list\default.html`文件，用以渲染`header-top.html`：

```html
{{ $image := partialCached "helper/image" (dict "Context" . "Type" "articleList") .RelPermalink "articleList" }}
<article class="{{ if $image.exists }}has-image{{ end }}">
    {{ partial "article/components/header-top" . }}
</article>
```

到这里，置顶效果就结束了，如果后续有其他方法，我会更新到下面。


## 附录

### 参考

- [🎉首页添加欢迎横幅 - linsnow博客](https://linsnow.cn/posts/bloglab/hugo-stack/modify-hugo/)

- [Weight - Hugo Docs](https://gohugo.io/methods/page/weight/)

- [if - Hugo Docs](https://hugo.opendocs.io/zh-cn/functions/go-template/if/)

### 版权信息

本文原载于 [彩虹兔の博客](https://cai-hong-tu-blog.pages.dev/)，遵循 CC BY-NC-SA 4.0 协议，复制请保留原文出处。
