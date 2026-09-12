# SEO keyword map (silent printing)

> Maintainer-only checklist. Do **not** link this from README / public guide copy.
> Public pages should read like engineering notes — never say “search intent”, “SEO”, or “keyword”.

Prefer one primary topic per page.

## English — primary intents

| Intent | Target page | Priority |
|---|---|---|
| silent printing web / browser silent print | `README.md`, `how-silent-printing-works.md` | P0 |
| window.print without dialog / silent | `window-print-vs-silent-print.md` | P0 |
| chrome localhost websocket 127.0.0.1 print | `chrome-local-network-access.md` | P0 |
| vue silent print / react silent print | `vue-react-silent-print.md` | P0 |
| html css silent print | `html-css-silent-print.md` | P0 |
| qz tray vs jsprintmanager | `qz-vs-jspm-vs-web-print-pdf.md` | P0 |
| lodop alternative | `lodop-alternatives.md` | P0 |
| batch label printing web | `batch-label-printing.md` | P1 |
| remote silent print / wms print | `remote-silent-print.md` | P1 |
| choose silent print stack | `choose-silent-print-stack.md` | P1 |
| hiprint alternative | `hiprint-alternatives.md` | P1 |
| thermal receipt print from browser | `thermal-receipt-silent-print.md` | P1 |
| qz tray alternative | `qz-tray-alternatives.md` | P1 |
| jsprintmanager alternative | `jsprintmanager-alternatives.md` | P1 |

## 中文 — 主意图

| 意图 | 目标页 | 优先级 |
|---|---|---|
| 网页静默打印 / 浏览器静默打印 | `README.zh-CN.md`, `how-silent-printing-works.zh-CN.md` | P0 |
| window.print 不弹窗 / 静默 | `window-print-vs-silent-print.zh-CN.md` | P0 |
| Chrome 127.0.0.1 WebSocket 打印失败 | `chrome-local-network-access.zh-CN.md` | P0 |
| Vue 静默打印 / React 静默打印 | `vue-react-silent-print.zh-CN.md` | P0 |
| HTML CSS 静默打印 | `html-css-silent-print.zh-CN.md` | P0 |
| Lodop 替代 / C-Lodop 替代 | `lodop-alternatives.zh-CN.md` | P0 |
| QZ Tray 对比 / 替代 | `qz-vs-jspm-vs-web-print-pdf.zh-CN.md`, `qz-tray-alternatives.zh-CN.md` | P0 |
| hiprint 替代 | `hiprint-alternatives.zh-CN.md` | P1 |
| 面单批量打印 网页 | `batch-label-printing.zh-CN.md` | P1 |
| 远程静默打印 / WMS 打印 | `remote-silent-print.zh-CN.md` | P1 |
| 热敏小票 网页打印 | `thermal-receipt-silent-print.zh-CN.md` | P1 |
| JSPrintManager 替代 | `jsprintmanager-alternatives.zh-CN.md` | P1 |

## Distribution rule (critical)

GitHub Markdown is weakly indexed for commercial queries.
**Every P0/P1 page must also ship on webprintpdf.com** with the same intent title, then link back to this repo for trust.

## On-page rules

1. H1 ≈ primary keyword
2. First 2 sentences answer the query directly
3. Internal links to 2–4 sibling guides + main README
4. One clear CTA path to product only where naturally relevant
5. No keyword stuffing; keep factual tone
