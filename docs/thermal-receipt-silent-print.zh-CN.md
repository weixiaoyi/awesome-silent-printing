# 浏览器热敏小票静默打印

**从网页打热敏小票** 通常必须静默：收银与厨打不能每张票都点一遍打印框。

## 什么行得通

1. **本地打印桥接** — 简单小票可用 HTML/图片/PDF；要完整控设备则用 ESC/POS raw
2. **厂商 SDK** — Epson / Star 类网口机（页面连打印机或厂商服务）
3. **桌面壳** — 若你们发 POS Electron，可用壳内静默打印

## 什么行不通

- 把 `window.print()` 当生产静默路径
- 只有 PDF 下载库、没有本机打印 Agent
- 以为写个「80mm CSS 页」就能切刀、弹钱箱——没有 raw 往往做不到

## 架构怎么选

| 需求 | 更倾向 |
|---|---|
| Logo + 可变 HTML 版式，量不大 | 经本地 Agent 打 HTML |
| 高频厨打、切刀、钱箱、蜂鸣 | QZ / JSPM / 厂商 SDK 上的 ESC/POS raw |
| 局域网里只有 Epson/Star | 厂商 ePOS / webPRNT 一类 SDK |
| 已有 POS 桌面应用 | Electron 静默打印 |

## 模板建议

- 固定宽度版式（如 **58mm / 80mm**）
- 条码高对比；用你们现场的扫码枪实测
- 少用沉重 CSS Grid；热敏驱动很挑剔
- 切刀 / 钱箱指令**只**在支持 raw 的栈上测
- ESC/POS 注意 CJK / 重音字符的代码页
- 驱动或 Agent 升级后打一张校准票

## 前台 vs 后厨

| | 前台收银 | 后厨 |
|---|---|---|
| 延迟容忍 | 低 | 极低 |
| 常见载荷 | HTML 或 ESC/POS | 多为 ESC/POS |
| 失败体验 | 给收银重试 | 自动重试 + 大声告警 |
| 多打印机 | 小票 + 面单 | 按档口 / 菜品路由 |

## 相关

- [网页批量 / 面单打印](batch-label-printing.zh-CN.md)
- [用 HTML/CSS 做静默打印](html-css-silent-print.zh-CN.md)
- [如何选型静默打印方案](choose-silent-print-stack.zh-CN.md)
- 工具列表：[Awesome Silent Printing 中文](../README.zh-CN.md)
