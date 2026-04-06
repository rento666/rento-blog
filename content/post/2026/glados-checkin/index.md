---
title: '利用 Cloudflare Workers 实现 Glados 自动签到'
description: 本项目实现 GLaDOS 每日自动签到，支持多账号、企业微信机器人通知，通过 Cloudflare Workers 免费部署，北京时间每日 8:00 自动执行，无需服务器、无需挂机。

date: 2026-04-05T16:48:42-07:00
lastmod: 2026-04-06T10:51:35-07:00
categories: ["学习笔记"]
tags: ["cloudflare", "glados"]
---

## 📋 准备工作（提前备好）
1. **免费 Cloudflare 账号**
   注册地址：https://dash.cloudflare.com/
2. **GLaDOS 账号 Cookie**（https://glados.rocks/）通过F12查看一个请求的Cookie
3. **可选**：企业微信/wxpusher/pushplus通知（用于接收签到成功/失败通知），推荐使用企业微信机器人密钥


## 🚀 全流程部署步骤
### 步骤 1：登录 Cloudflare 控制台
1. 打开 https://dash.cloudflare.com/
2. 输入你的账号密码完成登录
3. 进入左侧菜单 **Workers & Pages**（Workers 与页面）


### 步骤 2：创建新的 Worker 应用
1. 点击 **创建应用程序**
2. 选择 **创建 Worker**
3. 给 Worker 起个名字（例如 `glados-checkin`）
4. 点击 **部署**，完成初始创建


### 步骤 3：替换为完整签到代码
1. 创建完成后，点击 **快速编辑**（进入代码编辑器）
2. **清空编辑器里默认的所有代码**
3. 粘贴下方 **完整版代码**（支持手动触发 + Cron 定时 + 企业微信/wxpusher/pushplus通知）：

```javascript
/**
 * Cloudflare Workers 版 Glados 自动签到
 * 部署后可通过访问 Worker URL 触发签到，也可配置 Cron 定时触发
 */
async function glados(env) {
  const notice = [];
  // 从 Workers 环境变量获取 GLADOS Cookie（多行分隔多个账号）
  if (!env.GLADOS) return notice;

  const cookies = env.GLADOS.split('\n').filter(Boolean); // 过滤空行
  for (const cookie of cookies) {
    try {
      const commonHeaders = {
        'cookie': cookie,
        'referer': 'https://glados.cloud/console/checkin',
        'user-agent': 'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)',
      };

      // 执行签到请求
      const checkinRes = await fetch('https://glados.cloud/api/user/checkin', {
        method: 'POST',
        headers: { ...commonHeaders, 'content-type': 'application/json' },
        body: '{"token":"glados.cloud"}',
      });
      const checkinData = await checkinRes.json();
      if (checkinData?.code) throw new Error(checkinData?.message || '签到接口返回错误');

      // 获取账号剩余天数
      const statusRes = await fetch('https://glados.cloud/api/user/status', {
        method: 'GET',
        headers: commonHeaders,
      });
      const statusData = await statusRes.json();
      if (statusData?.code) throw new Error(statusData?.message || '状态接口返回错误');

      // 拼接签到成功通知
      notice.push(
        `✅ 账号签到成功`,
        `📢 ${checkinData.message}`,
        `⏳ 剩余天数：${Number(statusData.data.leftDays).toFixed(0)} 天`
      );
    } catch (error) {
      notice.push(`❌ 签到失败`, `原因：${error.message}`);
    }
  }
  return notice;
}

async function notify(notice, env) {
  // 从 Workers 环境变量获取通知配置
  if (!env.NOTIFY || !notice || notice.length === 0) return;

  const notifyOptions = env.NOTIFY.split('\n').filter(Boolean);
  for (const option of notifyOptions) {
    try {
      if (option.startsWith('console:')) {
        // 日志输出到 Cloudflare Workers 控制台
        console.log('📩 签到结果：\n' + notice.join('\n'));
      } else if (option.startsWith('wxpusher:')) {
        // 微信推送 WxPusher
        const [, appToken, ...uids] = option.split(':');
        await fetch('https://wxpusher.zjiecode.com/api/send/message', {
          method: 'POST',
          headers: { 'content-type': 'application/json' },
          body: JSON.stringify({
            appToken: appToken,
            summary: notice[0],
            content: notice.join('<br>'),
            contentType: 3,
            uids: uids.filter(Boolean),
          }),
        });
      } else if (option.startsWith('pushplus:')) {
        // 推送加 PushPlus
        const [, token] = option.split(':');
        await fetch('https://www.pushplus.plus/send', {
          method: 'POST',
          headers: { 'content-type': 'application/json' },
          body: JSON.stringify({
            token: token,
            title: notice[0],
            content: notice.join('<br>'),
            template: 'markdown',
          }),
        });
      } else if (option.startsWith('qyweixin:')) {
        // 企业微信机器人
        const [, robotKey] = option.split(':');
        await fetch(`https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=${robotKey}`, {
          method: 'POST',
          headers: { 'content-type': 'application/json' },
          body: JSON.stringify({
            msgtype: 'markdown',
            markdown: { content: notice.join('\n') },
          }),
        });
      } else {
        // 兜底使用 PushPlus（直接填 token 场景）
        await fetch('https://www.pushplus.plus/send', {
          method: 'POST',
          headers: { 'content-type': 'application/json' },
          body: JSON.stringify({
            token: option,
            title: notice[0],
            content: notice.join('<br>'),
            template: 'markdown',
          }),
        });
      }
    } catch (error) {
      console.error('通知发送失败：', error);
      throw error;
    }
  }
}

// Workers 核心请求处理函数
async function handleRequest(request, env) {
  try {
    const checkinNotice = await glados(env);
    await notify(checkinNotice, env);
    // 返回友好的 JSON 响应
    return new Response(JSON.stringify({
      success: true,
      message: '签到流程执行完成',
      notice: checkinNotice
    }), {
      headers: { 'Content-Type': 'application/json; charset=utf-8' },
      status: 200
    });
  } catch (error) {
    // 异常响应
    return new Response(JSON.stringify({
      success: false,
      message: '签到流程执行失败',
      error: error.message || error.toString()
    }), {
      headers: { 'Content-Type': 'application/json; charset=utf-8' },
      status: 500
    });
  }
}

// 注册 Workers Fetch 事件（入口）
export default {
  async fetch(request, env, ctx) {
    return handleRequest(request, env);
  },
  async scheduled(event, env, ctx) {
    handleRequest("", env);
  }
};
```

4. 点击右上角 **部署**


### 步骤 4：配置环境变量（核心！）

![环境变量](https://files.seeusercontent.com/2026/04/06/Qxa0/20260406170722223.png)

1. 退出代码编辑器，回到 Worker 详情页
2. 点击顶部 **设置** → 选择 **变量和机密**
3. 点击 **添加**，添加以下 2 个变量：

| 变量名 | 必填 | 填写内容 | 示例 |
|--------|------|----------|------|
| `GLADOS` | ✅ 是 | GLaDOS 账号 Cookie（多行=多账号） | koa:sess=xxx |
| `NOTIFY` | ❌ 可选 | 通知渠道 | `qyweixin:企业微信机器人key` <br> `console:`（仅控制台日志） |

#### 🔑 如何获取 GLaDOS Cookie
1. 打开 https://glados.rocks/ 并登录
2. 按 `F12` → 打开 **Application/应用** → **Cookies**
3. 复制完整的 Cookie 值（一长串字符串）

#### 🔑 如何获取企业微信机器人 Key
1. 企业微信创建群聊 → 群设置 → 添加机器人
2. 复制机器人 **Webhook 链接** 最后的 key 部分
3. 变量填写格式：`qyweixin:你复制的key`

4. 变量添加完成后，点击 **保存**


### 步骤 5：设置定时任务（北京时间 8:00 执行）

![定时任务](https://files.seeusercontent.com/2026/04/06/f3Ss/20260406170823927.png)

1. 回到 Worker 详情页，点击顶部 **触发器**
2. 找到 **Cron Triggers** → 点击 **添加 Cron 触发器**
3. 输入官方标准规则（**对应每天北京时间 8:00**）：
   ```cron
   0 0 * * *
   ```
4. 点击 **添加触发器** → 完成定时配置

> 原理：Cloudflare 仅支持 UTC 时区，`0 0 * * *` = UTC 0 点 = **北京时间 8 点**


### ✅ 测试是否部署成功
#### 方式 1：手动触发测试
1. 在 Worker 详情页，点击 **发送**（默认路由）
2. 等待几秒，页面返回签到结果 JSON
3. 查看企业微信是否收到通知

#### 方式 2：查看执行日志
1. 点击 Worker 详情页 **日志**
2. 可查看签到记录、报错信息

## ⚠️ 重要注意事项
1. **Cookie 有效期**
   GLaDOS Cookie 会过期，失效后重新复制更新环境变量，并部署Worker即可生效
2. **免费额度足够**
   Cloudflare 免费版每日 10 万次请求，完全满足每日签到

---

## 📞 常见问题
1. **签到失败**
   → 检查 Cookie 是否正确、是否登录真站、Cookie 是否过期
2. **企业微信收不到通知**
   → 检查机器人 key 是否正确、变量格式是否为 `qyweixin:xxx`
3. **Cron 不执行**
   → 检查规则是否为 `0 0 * * *`、是否保存触发器
4. **代码报错**
   → 检查环境变量是否填写、是否有多余空行

---

## 🎯 总结
1. 登录 Cloudflare → 创建 Worker
2. 粘贴代码 → 配置 Cookie + 通知变量
3. 添加 Cron 规则 `0 0 * * *`（每日北京时间 8 点）
4. 手动测试 → 完成全自动签到！
