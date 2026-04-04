---

title: "Cloudflare Workers 项目推荐"
slug: " cf-workers-recommend"
description: "推荐一些基于 Cloudflare Workers 的项目！"
date: "2026-03-23T22:01:22+08:00"
lastmod: "2026-03-23T22:01:22+08:00"

categories: ["学习笔记"]
tags: ["cloudflare"]

---

## 1. Saas Admin Template

用 Astro、shadcn/ui 和 Cloudflare Workers 构建的管理仪表盘模板

{{<link title="Saas Admin Template" link="https://github.com/cloudflare/templates/tree/v.8.1.0/saas-admin-template" cover="https://plink.159499.xyz/xlEHkMNsNC2HsQDR" >}}

```bash
npm create cloudflare@latest -- --template=cloudflare/templates/saas-admin-template
```

## 2. React Router + Hono Fullstack App

这是一个基于 Cloudflare Workers 的现代化全栈模板，后端 API 使用 Hono，前端路由使用 React Router，组件美观易用，并使用 Tailwind CSS 进行样式设计。

{{<link title="React Router + Hono Fullstack App" link="https://github.com/cloudflare/templates/tree/v.8.1.0/react-router-hono-fullstack-template" cover="https://plink.159499.xyz/18MeMncFDVyGdFa9" >}}

```bash
npm create cloudflare@latest -- --template=cloudflare/templates/react-router-hono-fullstack-template
```

## 3. To-Do List App

一个简单的待办事项应用，由 Cloudflare Workers Assets 和 Remix 构建。

{{<link title="To-Do List App" link="https://github.com/cloudflare/templates/tree/v.8.1.0/to-do-list-kv-template" cover="https://plink.159499.xyz/Du9CxLrn0HNoBXLI" >}}

```bash
npm create cloudflare@latest -- --template=cloudflare/templates/to-do-list-kv-template
```

## 参考

- [Cloudflare Workers Templates](https://workers.new/templates/)