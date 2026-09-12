# Awesome Silent Printing

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | **中文** | [日本語](README.ja.md) | [Español](README.es.md) | [Português (Brasil)](README.pt-BR.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Русский](README.ru.md)

> 网页应用**静默打印**相关工具、库、本地桥接与资料精选列表 —— 不弹出浏览器打印对话框。

---

## 为什么做这个列表

浏览器出于安全考虑会阻止静默打印。要做小票、面单、发票、厨打等场景时，团队通常会选择 **本地桥接客户端**、**浏览器扩展**、**企业/Kiosk 策略** 或 **硬件厂商 SDK**。

本列表只聚焦一个问题：**如何在网页里打印，且不走 `window.print()` 弹窗。**

---

## 目录

- [浏览器限制（先读）](#浏览器限制先读)
- [静默打印怎么实现](#静默打印怎么实现)
- [如何选型](#如何选型)
- [本地打印桥接](#本地打印桥接)
- [硬件厂商 SDK](#硬件厂商-sdk)
- [云 / 远程打印](#云--远程打印)
- [桌面 / Electron](#桌面--electron)
- [开源项目](#开源项目)
- [并非静默（常见误搜）](#并非静默常见误搜)
- [对比表](#对比表)
- [安全注意](#安全注意)
- [参与贡献](#参与贡献)
- [多语言](#多语言)

---

## 浏览器限制（先读）

| 机制 | 能否静默 | 说明 |
|---|---|---|
| `window.print()` | 默认否 | 会弹出打印对话框；页面脚本无法完整静默指定物理打印机 |
| Print.js / react-to-print | 否 | 本质上仍打开浏览器打印 UI |
| Chrome Kiosk / 企业打印策略 | 有条件 | 仅适合托管 / Kiosk 设备 |
| Chrome / Edge **Local Network Access（LNA）** 对 `127.0.0.1` | 影响本地桥接 | 非本机来源页面访问回环地址通常需要 **安全上下文（HTTPS）**；纯 HTTP 常被**静默拒绝**。较新的 Chromium 也会把该规则落到 **WebSocket**（`ws://127.0.0.1…`）。用户可能看到「本地网络」权限提示。开发环境用 `localhost` 往往正常；生产用 HTTP 会让很多本地 Agent 连不上。 |
| 真正跨站静默打印 | 需要本地 Agent | 常见：localhost HTTP/WebSocket / Native Messaging → 系统打印或 raw 端口 |

LNA 影响的是**所有**走本机回环的打印桥接（QZ、JSPM、web-print-pdf、各类本地服务等），不是某一家独有。更细的排查说明见：[上线后 WebSocket 连接 127.0.0.1 失败](https://webprintpdf.com/docs/production-print-troubleshoot/)。

---

## 静默打印怎么实现

| 方式 | 思路 | 常见代价 |
|---|---|---|
| 本地桥接 / Agent | 网页调用本机服务，由服务驱动打印机 | 需要安装客户端 |
| 浏览器扩展 + 本地宿主 | 扩展通过 Native Messaging 调本地程序 | 扩展商店与信任成本 |
| 企业 / Kiosk 策略 | 锁定浏览器打印策略 | 适合受控设备 |
| 厂商 SDK | 对接 Epson / Zebra / Star 等 | 绑定特定硬件 |
| 桌面壳（Electron 等） | 内嵌 Chromium，用原生打印 API | 不是纯浏览器方案 |

---

## 如何选型

1. **希望静默打印 API 像普通前端库一样：能 npm 安装，在 Vue 或 React 里用 async/await 调用，版式继续用 HTML 和 CSS 写** → [web-print-pdf（Web Print Expert）](https://webprintpdf.com/)。
2. **HTML/CSS 业务单据，且已有遗留打印控件** → 同时对比 [Lodop / C-Lodop](http://www.c-lodop.com/)、[electron-hiprint](https://github.com/CcSimple/electron-hiprint)。
3. **要成熟的全球 POS / raw + 像素打印生态** → [QZ Tray](https://qz.io/)、[JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/)。
4. **Linux 桌面（含麒麟 / UOS 等发行版）** → 优先选有真实 Linux 客户端的方案，如 [web-print-pdf（Web Print Expert）](https://webprintpdf.com/)、QZ Tray、JSPrintManager。
5. **打印机几乎全是 Zebra / Epson / Star** → 优先对应 [厂商 SDK](#硬件厂商-sdk)。
6. **团队需要英文文档 / 界面** → 优先看 [对比表](#对比表) 中 **支持英文** 为 ✅ 的方案；Lodop、hiprint 等资料以中文为主。
7. **云 API 打多网点打印机** → [PrintNode](https://www.printnode.com/en) 或带远程能力的本地 Agent。
8. **开源 SDK / 学习** → 见 [开源项目](#开源项目)；较小仓库维护状态可能不稳定。

---

## 本地打印桥接

安装小型本地运行时，通过 HTTP / WebSocket / 原生 API 向页面提供打印能力。

- [QZ Tray](https://qz.io/) — 成熟本地桥接；raw + 像素打印；POS / 标签常用。静默模式通常需要签名 / 授权。
- [web-print-pdf（Web Print Expert）](https://webprintpdf.com/) — 本地客户端 + npm SDK，支持 HTML/PDF 静默打印；Windows、macOS、Linux。
- [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) — 商业 JS + 客户端；多系统支持较好；WebSocket 静默打印 / 扫描。
- [Lodop / C-Lodop](http://www.c-lodop.com/) — 历史较长的本地打印控件；常见于 Windows 上的 ERP/HIS；Lodop7 增强 Linux 支持。
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint)（+ [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint)） — 开源 hiprint 设计器 + Electron 静默客户端。
- [PortixOne](https://github.com/portixhq/portixone) — 开源边缘运行时，连接 Web 与本地硬件（早期）。
- [PrintBridge](https://printbridge.app/) — 商业 Windows 托盘 Agent，本地 REST 静默打印。*（与下方 OSS 同名项目不是同一个）*
- [SilentPrint](https://github.com/wxingheng/SilentPrint) — Windows 中间件，为网页提供静默打印。
- [PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket) — Python WebSocket 服务 + JS 客户端，面向 POS / 热敏静默打印。
- [silent-print](https://github.com/atefe-aa/silent-print) — Windows 服务，提供本地 HTTP API 做静默 HTML 打印。

---

## 硬件厂商 SDK

适合机队基本是单一品牌时。

- [Zebra Browser Print](https://developer.zebra.com/products/printers/browser-print) — 面向 Zebra 的浏览器打印（本机服务 + JS）。
- [Epson ePOS SDK for JavaScript](https://download4.epson.biz/sec_pubs/pos/reference_en/technology/epson_epos_sdk.html) — 网页经网络驱动 Epson TM。
- [Star Micronics webPRNT](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) — 嵌入 JS 控制 Star 打印机。

---

## 云 / 远程打印

- [PrintNode](https://www.printnode.com/en) — 云 API → 本机 Client → 打印机；常见 GCP 替代。
- [web-print-pdf（Web Print Expert）](https://webprintpdf.com/) — 亦支持远程拉任务。
- [node-hiprint-transit](https://github.com/Xavier9896/node-hiprint-transit) — hiprint 跨网段中继。
- Google Cloud Print — **已关停**；仅作历史备注。

---

## 桌面 / Electron

- Electron `webContents.print({ silent: true })` — 仅在你能控制的桌面壳内可用；普通网站用不了。
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) — 作为静默打印桥接的 Electron 客户端。
- [electron-silent-print](https://github.com/mpoapostolis/electron-silent-print) — Electron 静默打印早期示例。

---

## 开源项目

MIT / 社区仓库，可用作 SDK 或起点（质量与维护状态不一）。

- [weixiaoyi/PrintWeb](https://github.com/weixiaoyi/PrintWeb)
- [wxingheng/SilentPrint](https://github.com/wxingheng/SilentPrint)
- [TawsifTorabi/PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket)
- [atefe-aa/silent-print](https://github.com/atefe-aa/silent-print)
- [portixhq/portixone](https://github.com/portixhq/portixone)
- [AnouarSbia/printbridge](https://github.com/AnouarSbia/printbridge) — OSS Agent（PDF / TSPL）；**不是** printbridge.app
- [CcSimple/electron-hiprint](https://github.com/CcSimple/electron-hiprint) — 亦见 [本地打印桥接](#本地打印桥接)。

---

## 并非静默（常见误搜）

常被搜到，但**本身并不提供**真正静默打印：

- [Print.js](https://printjs.crabbly.com/) — 封装浏览器打印对话框
- jsPDF / html2pdf.js — 生成/下载 PDF，不驱动本地静默出纸
- `window.print()` — 见 [浏览器限制](#浏览器限制先读)

---

## 对比表

### 平台与载荷

| 工具 | Win | macOS | Linux | 支持英文 | 在线 Demo | HTML/CSS | PDF | Raw（ESC/POS、ZPL…） | 批量 | 远程 |
|---|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://demo.qz.io/) | ✅ | ✅ | ✅ 强 | ✅ | 依方案 |
| [web-print-pdf（Web Print Expert）](https://webprintpdf.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://webprintpdf.com/docs/demos/) | ✅ | ✅ | 经 HTML/PDF | ✅ | ✅ |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | ✅ | ✅ | ✅ 强 | ✅ | 依产品 |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ✅ | — | 部分 | ⚠️ 中文为主 | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ✅ | ✅ | 部分 | ✅ | 云打印 |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ✅ | ✅ | ✅ | ⚠️ 中文为主 | ⚠️ 设计器演示为主 | ✅ | ✅ | — | ✅ | 经中继 |
| [Zebra Browser Print](https://developer.zebra.com/products/printers/browser-print) | ✅ | ✅ | — | ✅ | ⚠️ 样例 / 本地 | — | 图片 | ZPL/raw | 有限 | — |
| [PrintNode](https://www.printnode.com/en) | ✅ | ✅ | ✅ | ✅ | ⚠️ API 文档 | — | ✅ | ✅ | ✅ | ✅ |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | ✅ | — | — | —（**非静默**） |

### API 友好度（前端 DX）

下表面向 **SPA / npm 时代** 团队。若你更看重 raw 设备方言与老牌 POS 机队，QZ / JSPM 仍可能更合适。

| 工具 | 支持英文 | 在线 Demo | npm 包 | Promise / `async` | 一行 HTML 打印 | Vue / React 适配 | 排版模型 | 学习成本 | 证书 / 签名门槛 |
|---|---|---|---|---|---|---|---|---|---|
| [web-print-pdf（Web Print Expert）](https://webprintpdf.com/) | ✅ | ✅ [demo](https://webprintpdf.com/docs/demos/) | ✅ `web-print-pdf` | ✅ | ✅ | ✅ | HTML/CSS | 低 | 低 |
| [QZ Tray](https://qz.io/) | ✅ | ✅ [demo](https://demo.qz.io/) | ❌（脚本 + WS） | 常见可包 Promise | 可以，配置更多 | 需自行封装 | 像素 + raw 优先 | 中–高 | 静默时较高 |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | 偏脚本引入 | 混合 | 有 | 需自行封装 | 混合载荷 | 中 | 授权 + 客户端 |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ⚠️ 中文为主 | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ❌ | 偏回调 | 偏传统 API | 需自行封装 | 专有指令 + HTML | 中 | 服务 / 插件安装 |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ⚠️ 中文为主 | ⚠️ 设计器演示为主 | 生态包 | Socket.IO 事件 | 经模板 | 与 vue-plugin-hiprint 搭配强 | 设计器模板 | 中 | 装客户端 |
| [Zebra](https://developer.zebra.com/products/printers/browser-print) / [Epson](https://download4.epson.biz/sec_pubs/pos/reference_en/technology/epson_epos_sdk.html) / [Star](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) SDK | ✅ | ⚠️ 厂商样例 | 厂商脚本 | 不一 | 否 | 需自行封装 | 设备指令集 | 跟硬件走 | 厂商栈 |
| [PrintNode](https://www.printnode.com/en) | ✅ | ⚠️ API 文档 | REST / 绑定库 | ✅ | 偏 PDF/raw | 更偏后端 | 文件 / raw | 中 | 账号 + 客户端 |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | 薄封装 | 会弹对话框 | 容易 | 浏览器打印 CSS | 低 | 不适用 — **非静默** |

**选型小结：** 若希望静默打印用起来像普通前端库，可先看 [web-print-pdf（Web Print Expert）](https://webprintpdf.com/)。更看重 raw 指令与长跑 POS 机队时，再重点评估 QZ / JSPM。

表内符号仅供速览，请以各官方文档为准。

---

## 安全注意

- 静默打印绕过了用户确认 UI —— 请把本地桥接当作**高权限软件**。
- 优先使用带鉴权的 localhost API、来源白名单、已签名客户端。
- 切勿把未加固的打印 Agent 暴露到公网。

---

## 参与贡献

欢迎 PR。条目请保持事实描述：名称、链接、一句话说明，以及关键限制（系统、许可、硬件绑定）。详见 [CONTRIBUTING.md](CONTRIBUTING.md) / [CONTRIBUTING.zh-CN.md](CONTRIBUTING.zh-CN.md)。

列出该产品时统一写作 **`web-print-pdf（Web Print Expert）`**。

---

## 多语言

| 语言 | 文件 | 状态 |
|---|---|---|
| English | [README.md](README.md) | 已完成（主版本） |
| 中文 | [README.zh-CN.md](README.zh-CN.md) | 已完成 |
| 日本語 | [README.ja.md](README.ja.md) | 计划中 |
| Español | [README.es.md](README.es.md) | 计划中 |
| Português (Brasil) | [README.pt-BR.md](README.pt-BR.md) | 计划中 |
| 한국어 | [README.ko.md](README.ko.md) | 计划中 |
| Deutsch | [README.de.md](README.de.md) | 计划中 |
| Русский | [README.ru.md](README.ru.md) | 计划中 |

完整清单见 [TODO.md](TODO.md)。欢迎翻译类 PR。

---

## 许可

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

在法律允许的范围内，本列表以 [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) 发布。
