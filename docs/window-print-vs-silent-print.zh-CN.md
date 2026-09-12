# window.print 与静默打印

很多人会问：能不能调用 `window.print()` 却不弹框？  
短答案：**普通网页里不行。**

浏览器把打印当成需要用户确认的特权操作。打印框就是安全阀。封装 `window.print()` 的库（如 Print.js、react-to-print）最后仍然会进那个界面。

## 对比

| | `window.print()` / Print.js | 静默打印桥接 |
|---|---|---|
| 对话框 | 有 | 无 |
| JS 指定打印机 | 基本不行 | 可以（经本地 Agent） |
| 批量任务 | 差 | 本来就为队列准备 |
| 指定打印机 / 纸盒 / 纸张 | 靠用户点 | Agent API |
| 是否安装组件 | 否 | 通常要 |
| 安全模型 | 浏览器管控 | 本机高权限软件 |
| 非管控 SaaS 终端 | 可以（会弹框） | 需要装 Agent |

## 浪费迭代的误解

| 误解 | 现实 |
|---|---|
| 「一定有给 SaaS 用户用的 Chrome 开关」 | 策略/Flag 面向管控设备，不是公网访客 |
| 「服务端 Puppeteer 就是静默打印」 | 它产 PDF/图，不驱动用户工位上的打印机 |
| 「先下载 PDF 也差不多」 | 用户仍要手打；没有批量与指定打印机 |
| 「Electron 的静默 API 浏览器构建也能用」 | 那些 API 只存在于你发出去的桌面壳里 |

## 什么时候 `window.print()` 就够

- 偶尔、由用户主动触发的打印（用户本来就会确认）
- 法务 / 医疗等希望显式确认的流程
- 量小，不是 Kiosk / 仓配自动化
- 你坚决不想做本机安装

## 什么时候必须静默打印

- 面单、小票、厨打
- 无人值守或高频出纸
- SPA 流程里弹框会打断体验（扫码 → 打印 → 下一单）
- 一机多打（面单 / A4 / 小票）要由软件选择

## 迁移路径

1. **盘点**现状：HTML 截屏、CSS `@media print`、PDF，还是 raw。
2. 目标桥接能渲 HTML 时，**尽量保留 HTML/CSS 模板**；只有必须 ZPL/ESC/POS 的再改写。
3. 用 [如何选型](choose-silent-print-stack.zh-CN.md) 选定本地 Agent（或厂商 SDK / Electron）。
4. 把 `window.print()` 调用点换成 SDK（经 localhost 打 HTML/PDF/raw）。
5. 补上 **未安装 Agent** 的体验：探测连接、给安装入口、未就绪时禁用「打印」。
6. 站点上 **HTTPS**，并在干净工位验证 [访问 `127.0.0.1` 的 LNA](chrome-local-network-access.zh-CN.md)。
7. 先试点一个工位一周并打日志，再全量。

### 代码形态变化（最小）

之前：

```js
window.print();
```

之后（伪代码，各家 API 不同）：

```js
await printAgent.printHtml(document.getElementById('label').outerHTML, {
  printer: selectedPrinter,
});
```

## 上线前验收

- [ ] 正常路径不再弹打印框
- [ ] 正确打印机无需用户点击
- [ ] 单笔失败不会卡死整批
- [ ] 全新浏览器配置能走完 HTTPS + 本地网络授权一次
- [ ] Agent 离线给出可恢复错误，而不是一直转圈

## 相关

- [静默打印如何工作](how-silent-printing-works.zh-CN.md)
- [如何选型](choose-silent-print-stack.zh-CN.md)
- [Chrome 本地网络访问](chrome-local-network-access.zh-CN.md)
- [Vue / React 静默打印](vue-react-silent-print.zh-CN.md)
