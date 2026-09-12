# Chrome 本地网络访问与 127.0.0.1

## 现象

开发环境正常，生产环境失败，常见报错类似：

```text
WebSocket connection to 'ws://127.0.0.1:…' failed
Failed to connect to print agent
ERR_CONNECTION_REFUSED / net::ERR_FAILED
```

本机打印客户端明明开着，防火墙也查过了，**只有上线后的网站**连不上回环地址。

这是所有走本机桥接的静默打印里，最常见的「一上线就挂」故障之一。

## 原因

Chromium 的 **Local Network Access（LNA）** 会限制公网页访问用户的本地网络 / 回环。监听在 `127.0.0.1` 的打印 Agent 正好落在限制区里。

现场常见规则：

| 场景 | 典型结果 |
|---|---|
| 开发时站点是 `http://localhost` | 常常能通（特殊上下文） |
| 生产站是裸 **HTTP** | 常常被**静默拒绝** |
| 生产站是受信证书的 **HTTPS** | 可能弹出「本地网络」权限 |
| 新版 Chrome + 连回环的 WebSocket | `ws://127.0.0.1…` 同样受 LNA 约束 |
| Edge 等 Chromium 系 | 政策类似 |

所以「Agent 在跑」只是必要条件。**浏览器页面的源（origin）**还必须被允许访问回环。

## 决策流程

```text
页面能否访问 ws://127.0.0.1 / http://127.0.0.1？
│
├─ 不能，且站点是 HTTP
│     → 先上 HTTPS。没上之前别在 Agent 上空耗。
│
├─ 不能，且站点是 HTTPS
│     → 查本地网络权限 / 是否弹过提示
│     → 确认 Agent 端口与进程
│     → 同机用小工具测一下 WS/HTTP
│
└─ 能连上，但仍打不出
      → 打印机名、驱动、队列、模板 —— 不是 LNA
```

## 处理清单

1. **站点用 HTTPS**，且工位信任该证书（公网 CA 或你们内网 PKI）。用户未信任的自签证书会一直失败。
2. 首次打印时注意浏览器的 **本地网络** 权限提示，对你的源点允许。
3. 确认桌面打印客户端在监听 `127.0.0.1`，端口与 SDK 配置一致。
4. 用开发者工具 → Network：看 WS/HTTP 是被拦、被拒，还是复位。
5. **仅管控机队**：可用浏览器策略放宽检查。这不是公网 SaaS 终端用户的方案。
6. 把「允许本地网络权限」写进安装手册；否则售后会反复重装客户端。

## 怎么区分 LNA 和「Agent 挂了」

| 检查 | Agent 挂了 | LNA / 源问题 |
|---|---|---|
| 本机工具直连 `127.0.0.1:端口` | 失败 | 成功 |
| 同机、站点 HTTP | 可能失败 | 常常失败 |
| 同机、HTTPS + 已授权 | Agent 正常则成功 | 成功 |
| 换一个 Chrome 用户配置 | 相同 | 可能缺权限 |

## 谁会中招

所有走本机回环的打印桥接：QZ Tray、web-print-pdf、JSPrintManager、Lodop 类本地服务、自建 Agent。这不是某一家独有的 bug。

## 更细的排查

- 中文：[上线后连接 127.0.0.1 失败](https://webprintpdf.com/docs/production-print-troubleshoot/)
- English: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/)

## 相关

- [静默打印如何工作](how-silent-printing-works.zh-CN.md)
- [Vue / React 静默打印](vue-react-silent-print.zh-CN.md)
- [如何选型](choose-silent-print-stack.zh-CN.md)
