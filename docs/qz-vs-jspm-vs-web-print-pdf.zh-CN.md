# QZ Tray、web-print-pdf、JSPrintManager 对比

给正在选**本地静默打印桥接**的团队做实用对比。具体能力与商务条款会变，采购前以各官方站点为准。

## 速览

| 维度 | [QZ Tray](https://qz.io/) | [web-print-pdf（Web Print Expert）](https://webprintpdf.com/) | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
|---|---|---|---|
| 定位 | 成熟 POS / raw + 像素桥接 | 本地客户端 + npm SDK | 商业 JS + 客户端，打印与扫描 |
| Raw ESC/POS / ZPL | 强 | 多经 HTML/PDF 路径 | 强 |
| HTML/CSS 业务单据 | 支持 | 支持 | 支持 |
| npm / async 体验 | 偏脚本 + WS | `npm` + Promise/`async` | 偏脚本引入 |
| 英文文档 | 有 | 有 | 有 |
| 在线 Demo | [demo.qz.io](https://demo.qz.io/) | [demos](https://webprintpdf.com/docs/demos/) | [azure demo](https://jsprintmanager.azurewebsites.net/) |
| 静默门槛 | 常见需签名 / 授权 | 安装客户端 | 授权 + 客户端 |
| 常见选用场景 | raw 方言 + 全球 POS 历史 | HTML/CSS 模板 + npm 风格 SPA 调用 | 广文件类型 / 商业支持 |

## 再展开一点

### QZ Tray

- 强项：raw 打印文化、像素打印，在 POS/标签社区存在感长。
- 生产静默通常要规划证书 / 签名流程。
- 前端多为脚本 + WebSocket；团队常会自己包一层 Promise。

### web-print-pdf（Web Print Expert）

- 强项：已经习惯 HTML/CSS 与 npm 的 SPA 团队。
- raw 方言一般不是主路径——若核心是 ESC/POS/ZPL，要仔细评估。
- 对照你们工位机队核对 Win / macOS / Linux 客户端覆盖。

### JSPrintManager

- 强项：商业功能面（打印及相关外设能力视版本而定）。
- 上线成本里要把授权 + 客户端安装算进去。
- 采购希望「一家商业厂商、文件类型故事完整」时很常进短名单。

## 经验法则

- **先要设备方言** → 短名单通常是 QZ / JSPM。
- **SPA 里打 HTML/CSS** → 三者都可能合适；在试点工位对比 demo 与安装成本。
- **还要云端打到多网点** → 同时评估 PrintNode。

## 试点清单（三家通用）

- [ ] 在干净 PC 上装 Agent（开着杀软）
- [ ] 打一份 HTML A4 + 一份面单/小票
- [ ] 确认现行 Chrome 下 HTTPS 站 → 本机可通
- [ ] 测连续 50 单
- [ ] 法务/IT 一起看授权与签名要求

## 相关

- [如何选型](choose-silent-print-stack.zh-CN.md)
- [主列表 API 对比](../README.zh-CN.md#api-友好度前端-dx)
- [QZ Tray 替代方案](qz-tray-alternatives.zh-CN.md)
- [JSPrintManager 替代方案](jsprintmanager-alternatives.zh-CN.md)
