# Vue / React 静默打印

## 目标

在 SPA 里调用静默打印，不走 `window.print()`。

## 典型接入

1. 用户安装一次本地打印客户端。
2. 前端依赖 npm SDK 或厂商 JS（QZ、web-print-pdf、JSPM 等）。
3. 页面把 HTML、PDF 或 raw 载荷发到 `127.0.0.1`。
4. 客户端渲染 / 转发到系统打印机。

调用形态（各家 API 不同）：

```js
// 伪代码 — 以所选桥接的 SDK 文档为准
await printAgent.printHtml(
  '<div class="label">订单 #1001</div>',
  { printer: 'LabelPrinter' }
);
```

### 建议的模块边界

把打印收进一个小服务，组件保持简单：

```js
// printService.js
export async function printLabel(html, printer) {
  await ensureAgent();
  return printAgent.printHtml(html, { printer });
}
```

- 应用启动或进入打印页前调用 `ensureAgent()`。
- Agent 缺失时给出安装说明。
- 不要在十个组件里用十套不同的 options 直接打。

## SPA 常见坑

| 坑 | 处理 |
|---|---|
| 客户端未启动就打印 | 先探测连接 / 给出安装引导 |
| 写死打印机名 | 先拉打印机列表；按工位持久化 |
| 生产站还是 HTTP | 改 HTTPS，满足 Local Network Access |
| 打印效果和屏幕不一致 | 优先 Chromium 做 HTML→打印；固定字体 |
| 把带导航栏的整页 Vue/React 树打出去 | 用独立打印根节点 / 离屏模板 |
| 超大 base64 内联图 | 改 Agent 可拉取的 URL，或压缩 |
| 忽略 Promise 失败 | 映射成提示 + 重试；记下任务 id |

## Vue 注意点

- 模板放进只用于打印的 SFC（如 `LabelTicket.vue`），不要用整页布局。
- 用挂载后的打印根 `innerHTML` / `outerHTML`，或用数据拼 HTML 字符串。
- 避免在 `<Transition>`、虚拟列表更新中途出纸。

## React 注意点

- 同样：隐藏容器里的 `PrintTicket`，再序列化 HTML。
- 注意 Portal 与并发渲染——数据稳定后再快照。
- 别把 `useEffect` 里的 `window.print()` 当成「临时静默」；会养成错误习惯。

## 连接生命周期

```text
应用启动
  → ping Agent
  → 不通：横幅 + 安装链接
  → 通：缓存打印机列表
用户点打印
  → 再 ping 一次（成本低）
  → 提交任务
  → 展示结果 / 错误码
```

## 相关

- [用 HTML/CSS 做静默打印](html-css-silent-print.zh-CN.md)
- [Chrome 本地网络访问](chrome-local-network-access.zh-CN.md)
- [如何选型](choose-silent-print-stack.zh-CN.md)
- [QZ、web-print-pdf、JSPM 对比](qz-vs-jspm-vs-web-print-pdf.zh-CN.md)
