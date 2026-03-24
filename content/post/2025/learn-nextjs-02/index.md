---

title: "【02】学习Next.js框架之链接、导航、进阶路由"
slug: "learn-nextjs-02"
description: "本文介绍了Next.js的入门、路由、布局等知识，适合有一定React基础的人阅读。"
date: "2025-10-27T09:17:42+08:00"
lastmod: "2025-10-27T09:17:42+08:00"

categories: ["学习笔记"]
tags: ["nextjs"]
reaction: true
top: false

---

## 链接 `Link`

### 基础用法

```ts
import Link from "next/link";

export default function Home() {
    return (
        <>
            <h1>Welcome home!</h1>
            <Link href="/blog">Blog</Link>
            <Link href="/login" replace>Login</Link>
        <>
    );
}
```

- 上述代码，点击Blog后会跳转到`http://localhost:3000/blog`

- 点击Login后会替换当前历史条目，此时再点返回，就会回到首页。

### 高亮当前链接

```ts
// app/(auth)/layout.tsx
"use client";

import Link from "next/link";
import { usePathname } from "next/navigation";
import "./styles.css";

const navLinks = [
    { name: "Register", href: "/register" },
    { name: "Login", href: "/login" },
    { name: "Forgot Password", href: "/forgot-password" }
];

export default function AuthLayout({
    children,
}: {
    children: React.ReactNode;
}) {
    const pathname = usePathname();
    return (
        <div>
            { navLinks.map((link) => {
                const isActive = 
                    pathname === link.href || 
                    (pathname.startsWith(link.href) && link.href !== "/");
                return (
                    <Link 
                        classNam={isActive ? "font-bold mr-4" : "text-blue-500 mr-4"} 
                        href={link.href} 
                        key={link.name}
                    >
                        {link.name}
                    </Link>
                )
            }) }
            {children}
        </div>
    );
}

```

```css
/* app/(auth)/style.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 获取URL的搜索参数

诸如`http://localhost:3000/articles/123?lang=zh`之类的链接，如何获取`123`和`zh`呢？

可以通过`params`获取这个articleId，通过`searchParams`获取这个lang。

```ts
// app/articles/[articleId]/page.tsx
import Link from next/link";

export default async function NewsArticle({
    params,
    searchParams,
}: {
    params: Promise<{ articleId: string }>;
    searchParams: Promise<{ lang?: "en" | "zh" }>;
}) {
    const { articleId } = await params;
    const { lang = "en" } = await searchParams;;
    return (
        <div>
            <h1>News article {articleId}</h1>
            <p>Reading in {lang}</p>

            <div>
                <Link href={`/article/${articleId}?lang=en`}>English</Link>
                <Link href={`/article/${articleId}?lang=zh`}>Chinese</Link>
            </div>
        </div>
    )
}
```

#### 服务端组件使用async与await，客户端组件使用use

使用`"use client";`时，不能使用async与await，需要使用React的Hook：`use`。

```ts
"use client";

import Link from next/link";
import { use } from "react";

export default async function NewsArticle({
    params,
    searchParams,
}: {
    params: Promise<{ articleId: string }>;
    searchParams: Promise<{ lang?: "en" | "zh" }>;
}) {
    const { articleId } = use(params);
    const { lang = "en" } = use(searchParams);
    return (
        <div>
            <h1>News article {articleId}</h1>
            <p>Reading in {lang}</p>

            <div>
                <Link href={`/article/${articleId}?lang=en`}>English</Link>
                <Link href={`/article/${articleId}?lang=zh`}>Chinese</Link>
            </div>
        </div>
    )
}
```

- 所有的`page.tsx`都能获取到`params`与`searchParams`。
- 所有的`layout.tsx`仅能获取到`params`。

## 导航

### 程序化导航

使用Next.js的App Router来实现的。

```ts
"use client";

import { useRouter } from "next/navigation";

export default function OrderProduct() {
    const router = useRouter();
    const handleClick = () => {
        console.log("下单完成");
        router.push("/");           // 进入首页
        // router.replace("/");     // 重定向到首页
        // router.back();              // 返回上一页
        // router.forward();           // 在历史记录中向前移动
    };
    return (
        <>
            <h1>Order product</h1>
            <button onClick={handleClick}>下单</button>
        </>
    )
}

```

### 重定向函数
```ts
import { redirect }  from "next/navigation";

// ...

redirect("/products")       // 重定向到 http://localhost:3000/products 页面

// ...
```

## 模板

`template.tsx`类似于`layout.tsx`，因为它们也是在你的应用中**被多个页面共享的 UI**。

每当用户在共享模板的路由之间导航时，你会得到一个全新的开始：

- 一个新的模板组件实例被挂载<br>
- DOM 元素被重新创建<br>
- 状态被清除<br>
- 效果被重新同步


### 对比

```ts
"use client";

import { useState } from "react"; 

// ...

const [input, setInput] = useState("");

// ...

<input value={input} onChange={e => setInput(e.target.value)} />

<Link href="/login">Login</Link>
<Link href="/register">Register</Link>

// ...
```

- 在`app/(auth)/layout.tsx`中填写上述代码时，从login页面跳转到register页面时，input中的值会保留<br>
- 在`app/(auth)/template.tsx`中,跳转页面，input的值会恢复为默认空白值<br>
- 我们可以同时使用`layout.tsx`和`template.tsx`：`layout`会首先渲染，然后其子组件将会根据`template`组件的输出重新放置。<br>
-`template`不是一个常见的需求，`layout`应该是您共享UI的首选。

![layout & template](https://s2.loli.net/2025/10/27/kSucoLf2EaTNI4b.png)

## 加载页面

我们已经学习了`page.tsx`、`layout.tsx`、`template.tsx`、`not-found.tsx`，下面我们继续学习新的特殊文件：`loading.tsx`

```ts
export default function Loading() {
    return <h1>Loading</h1>;
}
```

一般如果使用加载页面的，都会为该页面做一个骨架加载器(skeleton loaders)或者其他

### 优点

- 当用户导航到新地方时，它会立即提供反馈。

这使您的应用程序感觉流畅且响应迅速，用户知道他们的点击确实产生了效果。

- Next.is 在加载新内容时保持共享布局的交互性。

即使主要内容尚未准备好，用户仍可使用导航菜单或侧边栏等功能。

## 错误处理

通过`error.tsx`的特殊文件实现。

```ts
// app/products/[productId]/reviews/[reviewId]/error.tsx
"use client";

export default function ErrorBoundary({ error }: { error: Error }) {
    return <div>{error.message}</div>
}
```

- `error.tsx`必须为客户端组件，即必须填写`"use client";`

```ts
// app/products/[productId]/reviews/[reviewId]/page.tsx

// ...
// 假设randomInt 只能取 0 或者 1
if(randomInt === 1) {
    throw new Error("Error loading review");
}

// ...
```

- 它会自动将路由段及其嵌套子组件包裹在React错误边界中

- 您可以使用文件系统层次结构为特定段创建自定义错误UI

- 它将错误隔离在受影响的段落中，同时保持应用程序其余部分的功能正常

- 它使您能够在不重新加载整个页面的情况下尝试从错误中恢复

### 组件层次结构

![组件层次结构](https://s2.loli.net/2025/10/29/9sgQHLceP2j8rwd.png)

### 从错误中恢复

`error.tsx`的参数还有一个reset方法，用来恢复客户端组件，如果`page.tsx`没有写`"use client";`，则可以在`error.tsx`中使用`useRouter`的Hook来重新加载页面。

```ts
"use client";

import { useRouter } from "next/navigation";
import { startTransition } from "react";

export default function ErrorBoundary({ error, reset }: { error: Error; reset: () => void }) {

    const router = useRouter();
    const reload = () => {
        startTransition(() => {
            router.refresh();
            reset();
        });
    };

    return (
        <div>
            <div>{error.message}</div>
            <button onClick={reload}>Try again</button>
        </div>
    )
}
```

### 处理全局错误

- 处理错误优先从同一目录下寻找`error.tsx`，若无，则会寻找父级目录，直至根目录下的`error.tsx`。

- 如果找到的是父级目录的`error.tsx`，则会替换父级`page.tsx`所展示的所有内容，包括所有的子组件内容。

- 如果本目录下的`layout.tsx`出错，则其会渲染父级目录下的`error.tsx`，而不是本目录下的`error.tsx`。

- 当应用程序的最高层级（根目录）的`layout.tsx`出现错误时，Next.js提供了`global-error.tsx`，该文件应该位于项目的`app`目录下。

- `global-error.tsx`仅在生产模式下工作

- return中需`<html><body> ... </body></html>`包裹。

- 由于无法再捕获任何错误，所以`global-error.tsx`请尽可能简单，最好包含HTML和CSS。

## 并行路由

是一种高级路由机制，可以在同一布局内同时渲染多个页面。

### 使用并行路由

- 在Next.js中，并行路由是通过一种称为插槽`slots`的功能来定义的。

- 插槽有助于以模块化的方式组织内容。

- 要创建一个插槽，我们使用`@folder`命名约定。

- 每个定义的插槽会自动成为其对应`layout.tsx`文件中的一个组件。

### 举例说明

想象一下，构建一个复杂的仪表板，同时在同一个页面显示用户分析、收入指标和通知等。

![并行路由的使用情景](https://s2.loli.net/2025/10/29/aBFq3N8QWyvrCzt.png)

#### 传统做法

传统的做法是为每个部分创建三个单独的组件，并将他们组织在`app/dashboard/layout.tsx`中。

![传统做法的layout.tsx代码](https://s2.loli.net/2025/10/29/HCfhEvpesrDS4Yk.png)

#### 并行路由做法

而并行路由做法是在`app/dashboard`文件夹内创建`@users/page.tsx`、`@revenue/page.tsx`、`@notifications/page.tsx`三个插槽。

![在dashboard文件夹内创建三个slots](https://s2.loli.net/2025/10/29/anMyeXzZRAmKFvJ.png)

在`app/dashboard/layout.tsx`中，无需导入，直接在参数中定义，然后使用即可。

![并行路由做法的部分layout.tsx代码](https://s2.loli.net/2025/10/29/lKHhGU5LEzkXMvC.png)

### slots无路由

你无法通过URL来访问到单独的插槽组件，例如`http:localhost:3000/dashboard/user`或者`http:localhost:3000/dashboard/@user`均为404 NOT FOUND。

### 使用案例

- 带有多个部分的仪表板

- 分屏界面

- 多窗格布局

- 复杂的管理界面

### 优势

- 并行路线非常适合将布局分成可管理的槽位(特别是当不同团队在不同的部分工作时)

- 独立的路由处理

布局中的每个槽位，例如用户、收入和通知，都可以处理其自身的加载和错误状态。

这种细粒度控制在页面不同部分加载速度不一或遇到独特错误的场景中尤为有用。

![一个出错，不影响其他](https://s2.loli.net/2025/10/29/XGMYfxrc2gOwDnT.png)

- 子导航

每个插槽本质上可以作为一个迷你应用程序，具备自己的导航和状态管理功能。

用户可以单独与每个部分进行交互，应用过滤器、排序数据或浏览页面而不影响其他部分。

![子导航之间共享](https://s2.loli.net/2025/10/29/ZJCnN29aDuFS4e7.png)


### 未匹配的路由

根据上图可知，`http://localhost:3000/dashboard`与`http:localhost:3000/dashboard/archived`的user、revenue是共享的。

由dashboard页面进入archived页面，没问题，那么在archived页面刷新一下，会如何呢？

答案是报错 404

因为：

- 从UI进行导航

当通过UI导航(例如点击链接)时，Next.js会继续显示之前未匹配槽位中的内容。

- 页面重新加载

Next.js会在每个未匹配的插槽中查找名为`default.tsx`的文件。该文件至关重要，当框架无法从当前URL检索到插槽的活动状态时，它将作为回退以渲染内容。

所以我们可以在`app/dashboard`、`app/dashboard/@users`、`app/dashboard/@revenue`文件夹内创建`default.tsx`，并将对应`page.tsx`文件的内容复制到里面。

## 条件路由

## src目录（可选）

`app`目录可直接放在项目的根目录下，也可放在`src`目录下。

放在`src`目录下就是单纯的方便项目结构整理，后续如果有公共组件，可以在`src`目录下创建`components`目录，用此目录存放公共组件。


## 附录
 
### 参考

- [7小时精通Next.js 15终极教程 (1小时15分钟到) | bilibili](https://www.bilibili.com/video/BV1kvPcekE3K/?share_source=copy_web&vd_source=1068a5fa51e306b8564255b5bf628111&t=4493)

### 版权信息

本文原载于 [彩虹兔の博客](https://cai-hong-tu-blog.pages.dev/)，遵循 CC BY-NC-SA 4.0 协议，复制请保留原文出处。