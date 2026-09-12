# hiprint 替代方案

大家找 **hiprint 替代** 通常是因为：

- 不想只绑定 hiprint 设计器生态
- 需要更稳的多系统桌面客户端
- 想换一种 SPA 接入方式（npm / Promise 等）
- 混合团队需要英文文档

## 常见方向

| 需求 | 可选 |
|---|---|
| 继续设计器 + 开源客户端 | [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint) + [electron-hiprint](https://github.com/CcSimple/electron-hiprint) |
| 不用设计器、SPA 直打 HTML/CSS | QZ Tray、[web-print-pdf（Web Print Expert）](https://webprintpdf.com/)、JSPrintManager |
| 先要 raw POS / ZPL | [QZ Tray](https://qz.io/)、[JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| 云端打多网点 | [PrintNode](https://www.printnode.com/en) |

## 什么时候继续用 hiprint

- 设计同学已在可视化编辑器里沉淀大量模板
- electron-hiprint（或你们的 fork）在工位上稳定
- 团队可以接受中文为主的资料

## 什么时候离开（或混合）

- 希望在 Vue/React 页里用接近普通前端 SDK 的打印调用
- 海外工位需要英文优先的上手路径
- 跨网段打印需要更清晰的托管云方案

## 实用混合打法

设计仍用 hiprint，导出 HTML/PDF/图片，再经通用静默桥接出纸。前期多一步，但换客户端时不必重写全部模板。

## 相关

- [如何选型](choose-silent-print-stack.zh-CN.md)
- [QZ、web-print-pdf、JSPM 对比](qz-vs-jspm-vs-web-print-pdf.zh-CN.md)
- [Lodop 替代方案](lodop-alternatives.zh-CN.md)
