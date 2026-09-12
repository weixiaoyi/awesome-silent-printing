# Lodop 替代方案

Lodop / C-Lodop 仍在很多 Windows 业务系统里。团队通常在这些需求下开始找替代：

- 更现代的 SPA 接入
- 更广的桌面系统支持（macOS / Linux 工位）
- 混合团队需要英文文档
- 在现行 Chromium 规则下，HTTPS + 本机访问更清晰

## 替换方向

| 需求 | 候选 |
|---|---|
| HTML/CSS 业务单据 | QZ Tray、[web-print-pdf（Web Print Expert）](https://webprintpdf.com/)、JSPrintManager、hiprint + electron-hiprint |
| Raw POS / 标签 | [QZ Tray](https://qz.io/)、[JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| 云端打多台打印机 | [PrintNode](https://www.printnode.com/en) |
| 继续用 Lodop | 若仍匹配，可看 Lodop7 / C-Lodop |

## 迁移为什么容易卡住

- 模板混杂 Lodop 专有指令与 HTML 碎片
- 打印机名、纸盒写死在老脚本里
- 医院 / ERP 怕旺季动「还能打」的路径

## 迁移建议

1. 盘点模板：HTML 还是专有指令。先数清有多少已经是「纯 HTML」。
2. 关键单据尽量改成 HTML/CSS；怪异 raw 放第二波。
3. 试点工位新旧客户端并行（不同端口）。
4. 全量推广前先解决 HTTPS / Local Network Access。
5. 培训售后认识新的「客户端没开」症状——它会取代老 ActiveX 时代的报错。

## 相关

- [如何选型](choose-silent-print-stack.zh-CN.md)
- [window.print 与静默打印](window-print-vs-silent-print.zh-CN.md)
- [QZ、web-print-pdf、JSPM 对比](qz-vs-jspm-vs-web-print-pdf.zh-CN.md)
