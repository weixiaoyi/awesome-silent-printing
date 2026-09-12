# 静默打印如何工作

浏览器的设计目标之一，就是**阻止**网页悄悄把任务打到物理打印机。所以 `window.print()` 会弹框，而「纯 JS 静默打印」在普通网页里通常走不通。

如果你要从 Web 打小票、面单、发票、厨打，需要换架构，而不是找一个浏览器隐藏开关。

## 真正的通用模式

几乎所有上线过的静默打印，都长这样：

```text
Web 应用（浏览器）
    → 本机 HTTP / WebSocket / Native Messaging
    → 本地 Agent / 厂商服务 / 桌面壳
    → 系统打印队列 或 设备 raw 端口
```

页面并不拥有打印机，**受信任的本机组件**才拥有。组件在工位装一次（或打进 Kiosk 镜像），网页再把它当成本地服务来调用。

### 为什么必须这样

| 顾虑 | 浏览器做法 | 静默打印做法 |
|---|---|---|
| 恶意站点疯狂打纸 | 禁止静默访问设备 | 用户主动安装已知 Agent |
| 打错打印机 | 强制弹框确认 | Agent + 指定打印机名 |
| Raw ESC/POS / ZPL | 页面拿不到 | Agent 或厂商 SDK 发字节 |
| 批量 / 无人值守 | 弹框打断流程 | Agent 或服务端队列 |

## 五种架构

| 路线 | 装什么 | 典型场景 | 取舍 |
|---|---|---|---|
| 本地打印桥接 | 桌面 Agent + JS SDK | ERP、WMS、POS、面单 | Web 静默最通用 |
| 扩展 + Native Host | 浏览器扩展 + 宿主程序 | 管控机队 | 商店审核与信任成本 |
| 企业 / Kiosk 策略 | 托管浏览器镜像 | 仅 Kiosk | 不适合公网 SaaS 用户 |
| 硬件厂商 SDK | 厂商服务或网口 API | Zebra / Epson / Star 机队 | 绑定硬件 |
| 桌面壳（Electron…） | 自研桌面应用 | 你能控制客户端时 | 不是纯浏览器方案 |

网上大量「Chrome 静默打印」讨论，最后多半落到 **本地桥接** 或 **厂商 SDK**。

## 一次打印任务里发生了什么

典型 HTML/PDF 桥接：

1. SPA 拼出 HTML（或 PDF URL / 字节）。
2. SDK 从 `https://你的站点` 连到 `ws://127.0.0.1:端口`（或 HTTP）。
3. Agent 接单，必要时用内嵌 Chromium 渲染 HTML。
4. Agent 交给系统打印队列，或打开 raw TCP/USB。
5. SDK 把成功 / 失败回给页面。

典型 raw POS 任务会跳过第 3 步的 HTML 渲染，直接发 ESC/POS 或 ZPL。

## 上线后常见故障

| 现象 | 常见原因 | 去哪查 |
|---|---|---|
| 本机开发正常，上线失败 | HTTP 站点被禁访问回环 | [Chrome 本地网络访问](chrome-local-network-access.zh-CN.md) |
| 能连上但不出纸 | 打印机名错 / 队列离线 | 先拉打印机列表；查系统队列 |
| 版式和屏幕不一致 | 渲染引擎不同 / 缺字体 | 固定字体；在目标系统实测 |
| 首单慢、后面正常 | Agent 冷启动 / 权限弹窗 | 登录后先探测连接 |
| POS 机偶发掉线 | 休眠、杀软、端口冲突 | Agent 做成服务；固定端口 |

## 为什么本机回环很关键

本地桥接通常监听 `127.0.0.1`。现行 Chromium 还有 **Local Network Access** 规则：公网/生产页往往要 **HTTPS**，才能打开 `ws://127.0.0.1…`。本机用 `http://localhost` 调试是另一套安全上下文，所以「我电脑上好好的」特别常见。

## 静默打印不是这些

- 只生成 PDF 下载（jsPDF、html2pdf…）——有用，但不是打印
- 打开系统打印框（Print.js、`window.print()`）
- 纯服务端渲染 PDF（Puppeteer）却没有打到**本机**打印机的路径
- 只在 Electron 里能静默，浏览器构建仍走 `window.print()`

## 安全底线

把本地 Agent 当高权限软件：

- 厂商支持鉴权 / 签名时优先用上
- 不要把 Agent 端口暴露到局域网或公网
- 限制可用打印机与模板；别让不信任输入「打印任意 URL」
- 记录 任务 id → 用户/工位 → 打印机 → 结果，方便审计

## 接下来

- [如何选型静默打印方案](choose-silent-print-stack.zh-CN.md)
- [window.print 与静默打印](window-print-vs-silent-print.zh-CN.md)
- [Chrome 本地网络访问与 127.0.0.1](chrome-local-network-access.zh-CN.md)
- 工具列表：[Awesome Silent Printing 中文](../README.zh-CN.md)
