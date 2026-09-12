# 用 HTML/CSS 做静默打印

## 为什么团队想走这条路

业务 UI 本来就是 HTML/CSS。若静默打印能复用同一套模板，前端迭代快，也不必为每张单据再养一套 ZPL/ESC/POS 技能。

## 两种排版模型

| 模型 | 优点 | 缺点 |
|---|---|---|
| 经本地 Chromium/Agent 的 HTML/CSS | Web 团队熟悉；还原度好 | 需要本机 Agent |
| Raw ESC/POS / ZPL | 设备控制精确 | 技能栈不同；绑硬件 |

很多产品是混合：A4 单据用 HTML，热敏小票用 raw。

## 这里的「打印 CSS」指什么

浏览器里的 `@media print` **并不是**静默路径——它仍会走 `window.print()`。走本地 Agent 时通常是：

1. 拼出独立 HTML 字符串（或 URL）。
2. 带上 Agent 需要的 CSS（内联、绝对 URL 或打包进去）。
3. 在 SDK 选项里传纸张 / 边距 / 打印机名。
4. 由 Agent 的引擎（常见为 Chromium）分页并交给系统队列。

### 模板清单

- [ ] 明确纸张尺寸（A4、100×150 mm、80 mm 卷纸…）
- [ ] 边距与实物耗材一致
- [ ] 字体已装在工位或已嵌入
- [ ] 条码/二维码用 SVG 或高清图（实测可扫）
- [ ] 表格最后一行不会被裁切
- [ ] 不依赖仅视口布局（`100vh` 一类陷阱）

## 实用建议

- 单独做 **打印专用** 样式，别盲目复用整站导航样式。
- 面单优先 `mm` / `in`；只写 `px` 容易随 DPI 漂。
- 若同时支持 Windows 与 Linux 工位，中文字体两边都要测。
- 迁模板时先预览单笔，再开批量。
- 控制图片体积；巨大 PNG 会拖垮批量吞吐。
- 精确切刀 / 钱箱指令，往往仍要在支持 raw 的桥接上发设备命令。

## 适合

发票、对账单、装箱单、A4 报表，以及大量用 HTML 渲的面单。

## 不太适合（考虑 raw / 厂商 SDK）

- 只要 ESC/POS、追求极致出纸速度的厨打
- 已在打印机固件里标准化 ZPL 的 Zebra 机队
- 除厂商 raw 外几乎没有可用驱动路径的设备

## 相关

- [Vue / React 静默打印](vue-react-silent-print.zh-CN.md)
- [网页批量 / 面单打印](batch-label-printing.zh-CN.md)
- [热敏小票静默打印](thermal-receipt-silent-print.zh-CN.md)
- [如何选型](choose-silent-print-stack.zh-CN.md)
