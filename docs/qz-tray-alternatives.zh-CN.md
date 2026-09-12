# QZ Tray 替代方案

搜 **QZ Tray 替代** 时，通常还是要浏览器静默打印，但想换 API 形态、价格、签名门槛，或更偏 HTML/CSS 的工作流。

## 什么时候继续用 QZ

- 主业是 raw ESC/POS / ZPL
- 已经投入过 QZ 签名与证书
- 需要全球 POS 长跑记录
- 团队已在生产里封装好 QZ WebSocket 调用

## 什么时候看别的

| 需求 | 可看 |
|---|---|
| 商业 JS 客户端、文件类型广 | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| SPA 里 npm 风格 HTML/CSS | [web-print-pdf（Web Print Expert）](https://webprintpdf.com/) 及其他支持 HTML 的桥接 |
| 云 API 打多台打印机 | [PrintNode](https://www.printnode.com/en) |
| 设计器模板路线 | hiprint + electron-hiprint |
| 单一硬件品牌 | Zebra / Epson / Star 厂商 SDK |

## 迁移注意

1. 先盘点哪些任务是 raw、哪些是 HTML/PDF——raw 重写成本最高。
2. 重新核对签名 / 授权假设；别默认下一家是「免签静默」。
3. 试点期间保留一台 QZ 工位。
4. 换 Agent 端口后，再验一次 Chrome Local Network Access。

## 直接对比

- [QZ Tray、web-print-pdf、JSPrintManager 对比](qz-vs-jspm-vs-web-print-pdf.zh-CN.md)
- [如何选型](choose-silent-print-stack.zh-CN.md)
