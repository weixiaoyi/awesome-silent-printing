# 浏览器静默打印总览

**浏览器静默打印**指：网页把任务打到本地打印机，且**不弹出**浏览器打印对话框。

这篇总览覆盖实际里最常碰到的几条路：

- 网页里不弹窗打印
- Chrome 打印不弹窗
- Vue / React 静默打印
- `window.print` 之外的做法

## 短答案

普通网站无法仅靠页面脚本，静默驱动任意物理打印机。你需要 **本地 Agent**、**厂商 SDK**、**Kiosk 策略** 或 **桌面壳**。

如果有人说「纯 JS 就能在 Chrome 里静默打任意打印机」，先问他装了哪个本机组件——那个组件才是真正的出纸路径。

## 一分钟心智模型

```text
页面（HTTPS）
  → 本机桥接
  → 系统打印队列或 raw 端口
  → 物理打印机
```

细节见：[静默打印如何工作](how-silent-printing-works.zh-CN.md)。

## 按场景继续读

| 你的情况 | 去读 |
|---|---|
| 想搞清架构 | [静默打印如何工作](how-silent-printing-works.zh-CN.md) |
| 要选型 | [如何选型静默打印方案](choose-silent-print-stack.zh-CN.md) |
| 从 `window.print` 迁出 | [window.print 与静默打印](window-print-vs-silent-print.zh-CN.md) |
| 上线后连不上 `127.0.0.1` | [Chrome 本地网络访问](chrome-local-network-access.zh-CN.md) |
| SPA 接入 | [Vue / React 静默打印](vue-react-silent-print.zh-CN.md) |
| HTML 模板 | [用 HTML/CSS 做静默打印](html-css-silent-print.zh-CN.md) |
| 面单 / 仓配批量 | [网页批量 / 面单打印](batch-label-printing.zh-CN.md) |
| WMS 推到工位 | [远程静默打印](remote-silent-print.zh-CN.md) |
| 小票 / 厨打 | [热敏小票静默打印](thermal-receipt-silent-print.zh-CN.md) |
| 对比主流桥接 | [QZ、web-print-pdf、JSPM 对比](qz-vs-jspm-vs-web-print-pdf.zh-CN.md) |

## 常见搜索 → 文章

| 大家在找什么 | 从这里开始 |
|---|---|
| 网页静默打印 / 浏览器静默打印 | 本页 |
| window.print 不弹窗 | [window.print 与静默打印](window-print-vs-silent-print.zh-CN.md) |
| Chrome 连 127.0.0.1 失败 | [Chrome LNA](chrome-local-network-access.zh-CN.md) |
| Lodop / hiprint / QZ 替代 | [Lodop](lodop-alternatives.zh-CN.md) · [hiprint](hiprint-alternatives.zh-CN.md) · [QZ](qz-tray-alternatives.zh-CN.md) · [JSPM](jsprintmanager-alternatives.zh-CN.md) |

## 工具列表

见精选列表：[Awesome Silent Printing 中文](../README.zh-CN.md)。
