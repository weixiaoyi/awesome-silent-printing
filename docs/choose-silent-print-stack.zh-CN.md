# 如何选型静默打印方案

当你已经确定需要**不弹浏览器打印框**，再用这份决策树选型。

## 快速决策

1. **你能控制桌面壳（Electron 等）**  
   直接用壳的静默打印 API，不必再上网页桥接。

2. **打印机几乎全是同一品牌（Zebra / Epson / Star）**  
   优先该厂商的浏览器 / 网口 SDK。少一层通用桥接，直接说设备方言。

3. **需要 raw ESC/POS / ZPL，以及全球 POS 长跑记录**  
   评估 [QZ Tray](https://qz.io/)、[JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/)。

4. **希望在 Vue / React 里用 HTML/CSS 打业务单据**  
   对比能接 HTML/PDF 的本地桥接——QZ Tray、[web-print-pdf（Web Print Expert）](https://webprintpdf.com/)、JSPrintManager、Lodop / C-Lodop、electron-hiprint——按系统覆盖、API 形态、授权成本选。

5. **多网点，云 API 打到本机打印机**  
   看 [PrintNode](https://www.printnode.com/en)，或支持远程拉任务的本地桥接。见 [远程静默打印](remote-silent-print.zh-CN.md)。

6. **已经在用 Lodop / C-Lodop**  
   能用先用；要更广桌面系统或换一种 SPA 接入方式时再规划迁移。见 [Lodop 替代方案](lodop-alternatives.zh-CN.md)。

## 选型记分表（采购前先填）

| 维度 | 你的需求 | 备注 |
|---|---|---|
| 载荷 | HTML / PDF / raw / 混合 | 比品牌名更能决定短名单 |
| 系统 | 仅 Win / +macOS / +Linux | 刷掉不少遗留控件 |
| 文档语言 | 英 / 中 / 双语 | 全球团队很关键 |
| 量级 | 少量 / 批量 / 仓配 | 决定队列与重试 |
| 安装摩擦 | IT 统装 / 终端自助 | 签名、杀软、权限 |
| 远程 | 仅同局域网 / 多网点 | 云方案 vs Agent 拉任务 |
| 预算 | 开源 / 商业授权 | 把支持成本算进去 |

## 一周试点计划

1. 短名单只留 **两个** 候选，不要五个一起试。
2. 同一台工位机装两个 Agent。
3. 打同一套三份样张：一份 A4 HTML、一份面单、一份边界样张（中文 + 条码）。
4. 记录：安装耗时、首单成功时间、失败提示、连续 50 单表现。
5. 故意踩一次 HTTPS / LNA，再按手册修好——让运维会排。
6. 留下赢家，卸掉输家，避免端口打架。

## 危险信号

- 厂商给不出在线 demo，也画不清本机架构
- 所谓「静默」其实只是下载 PDF
- 对生产站 HTTPS → 本机 Agent 的 Chrome LNA 没有说法
- 团队只会 HTML/CSS 却被推进纯 raw 栈（或反过来）

## 比品牌名更重要的问题

| 问题 | 为什么重要 |
|---|---|
| HTML/CSS 还是 raw 指令？ | 决定前端原生还是设备原生路线 |
| 只要 Windows，还是也要 macOS/Linux？ | 刷掉不少遗留控件 |
| 全球团队需要英文文档吗？ | 过滤以中文资料为主的方案 |
| 要不要批量 / 队列？ | 面单、仓配高频场景 |
| 生产站 HTTPS → 本机 Agent？ | Chrome Local Network Access |

## 相关文章

- [静默打印如何工作](how-silent-printing-works.zh-CN.md)
- [QZ、web-print-pdf、JSPM 对比](qz-vs-jspm-vs-web-print-pdf.zh-CN.md)
- [Vue / React 静默打印](vue-react-silent-print.zh-CN.md)
- 工具列表：[Awesome Silent Printing 中文](../README.zh-CN.md)
