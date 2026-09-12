# JSPrintManager 替代方案

找 **JSPrintManager 替代**，常见原因是价格、API 形态，或想换一套更偏 HTML/CSS 的前端工作流，同时仍要静默打印。

## 什么时候继续用

- 依赖 JSPM 较广的文件 / 打印 / 扫描能力
- 商业支持与多系统客户端覆盖最重要
- 采购已把 Neodynamic 授权标准化

## 按需求看选项

| 需求 | 选项 |
|---|---|
| 偏 raw 的 POS 生态 | [QZ Tray](https://qz.io/) |
| SPA 里 npm 风格 HTML/CSS | [web-print-pdf（Web Print Expert）](https://webprintpdf.com/) 及其他支持 HTML 的桥接 |
| 云打印路由 | [PrintNode](https://www.printnode.com/en) |
| 开源设计器 + Electron 客户端 | hiprint + electron-hiprint |
| 单一硬件品牌 | Zebra / Epson / Star 厂商 SDK |

## 迁移注意

1. 列出你们真正调用的 JSPM API（打印 / 扫描 / 文件类型）。
2. 映射到候选方案最接近的 API——会有胶水代码。
3. 重新算授权 + 支持成本；「SDK 更便宜」可能输在售后工时上。
4. 开着杀软在一台工位试点；商业客户端偶尔会触发一次告警。

## 相关

- [QZ、web-print-pdf、JSPM 对比](qz-vs-jspm-vs-web-print-pdf.zh-CN.md)
- [如何选型](choose-silent-print-stack.zh-CN.md)
- [Vue / React 静默打印](vue-react-silent-print.zh-CN.md)
