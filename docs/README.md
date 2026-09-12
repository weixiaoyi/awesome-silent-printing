# Guides

Practical notes on printing from the web **without** a browser dialog.

## English

### Start here

| Guide | About |
|---|---|
| [Browser silent print](browser-silent-print.md) | What “silent print” means in the browser, and where to go next |
| [How silent printing works](how-silent-printing-works.md) | Architecture overview |
| [Choose a silent print stack](choose-silent-print-stack.md) | Decision tree for picking a stack |

### Comparisons & migrations

| Guide | About |
|---|---|
| [window.print vs silent print](window-print-vs-silent-print.md) | Why `window.print()` always shows a dialog |
| [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.md) | Side-by-side of common bridges |
| [QZ Tray alternatives](qz-tray-alternatives.md) | Other options if QZ is not a fit |
| [JSPrintManager alternatives](jsprintmanager-alternatives.md) | Other options if JSPM is not a fit |
| [Lodop alternatives](lodop-alternatives.md) | Migration notes away from Lodop / C-Lodop |
| [hiprint alternatives](hiprint-alternatives.md) | Options around hiprint-style stacks |

### Implementation & ops

| Guide | About |
|---|---|
| [Chrome Local Network Access & 127.0.0.1](chrome-local-network-access.md) | HTTPS + localhost after Chrome LNA |
| [Silent print in Vue / React](vue-react-silent-print.md) | SPA integration patterns |
| [HTML/CSS silent print](html-css-silent-print.md) | Keep HTML/CSS templates with an agent |
| [Batch & label printing from the web](batch-label-printing.md) | Shipping labels and batch jobs |
| [Remote / server-pushed silent print](remote-silent-print.md) | WMS / remote push to a workstation |
| [Thermal receipt silent print](thermal-receipt-silent-print.md) | Receipt printers from the browser |

## 中文

### 从这里开始

| 文章 | 说明 |
|---|---|
| [浏览器静默打印总览](browser-silent-print.zh-CN.md) | 浏览器里「静默打印」是什么，以及接下来读哪篇 |
| [静默打印如何工作](how-silent-printing-works.zh-CN.md) | 架构科普 |
| [如何选型静默打印方案](choose-silent-print-stack.zh-CN.md) | 选型决策树 |

### 对比与迁移

| 文章 | 说明 |
|---|---|
| [window.print 与静默打印](window-print-vs-silent-print.zh-CN.md) | 为什么 `window.print()` 一定会弹窗 |
| [QZ Tray、web-print-pdf、JSPrintManager 对比](qz-vs-jspm-vs-web-print-pdf.zh-CN.md) | 常见桥接方案横评 |
| [QZ Tray 替代方案](qz-tray-alternatives.zh-CN.md) | QZ 不合适时的其他选项 |
| [JSPrintManager 替代方案](jsprintmanager-alternatives.zh-CN.md) | JSPM 不合适时的其他选项 |
| [Lodop 替代方案](lodop-alternatives.zh-CN.md) | 从 Lodop / C-Lodop 迁出 |
| [hiprint 替代方案](hiprint-alternatives.zh-CN.md) | hiprint 类方案周边选项 |

### 接入与运维

| 文章 | 说明 |
|---|---|
| [Chrome 本地网络访问与 127.0.0.1](chrome-local-network-access.zh-CN.md) | 上线 HTTPS 后连本机打印服务 |
| [Vue / React 静默打印](vue-react-silent-print.zh-CN.md) | SPA 接入方式 |
| [用 HTML/CSS 做静默打印](html-css-silent-print.zh-CN.md) | 继续用 HTML/CSS 模板 + Agent |
| [网页批量 / 面单打印](batch-label-printing.zh-CN.md) | 面单与批量出纸 |
| [远程 / 服务端推送静默打印](remote-silent-print.zh-CN.md) | WMS / 服务端推到工位 |
| [浏览器热敏小票静默打印](thermal-receipt-silent-print.zh-CN.md) | 热敏小票从网页出纸 |

## 日本語

### ここから始める

| ガイド | 内容 |
|---|---|
| [ブラウザサイレント印刷](browser-silent-print.ja.md) | ブラウザにおける「サイレント印刷」の意味と次に読む記事 |
| [サイレント印刷の仕組み](how-silent-printing-works.ja.md) | アーキテクチャ概要 |
| [サイレント印刷スタックの選び方](choose-silent-print-stack.ja.md) | スタック選定の決定ツリー |

### 比較と移行

| ガイド | 内容 |
|---|---|
| [window.print とサイレント印刷](window-print-vs-silent-print.ja.md) | `window.print()` が必ずダイアログを出す理由 |
| [QZ Tray、web-print-pdf、JSPrintManager 比較](qz-vs-jspm-vs-web-print-pdf.ja.md) | 主要ブリッジの横並び比較 |
| [QZ Tray 代替案](qz-tray-alternatives.ja.md) | QZ が合わない場合の選択肢 |
| [JSPrintManager 代替案](jsprintmanager-alternatives.ja.md) | JSPM が合わない場合の選択肢 |
| [Lodop 代替案](lodop-alternatives.ja.md) | Lodop / C-Lodop からの移行 |
| [hiprint 代替案](hiprint-alternatives.ja.md) | hiprint 系スタック周辺の選択肢 |

### 実装と運用

| ガイド | 内容 |
|---|---|
| [Chrome Local Network Access & 127.0.0.1](chrome-local-network-access.ja.md) | HTTPS 本番後の localhost 接続 |
| [Vue / React サイレント印刷](vue-react-silent-print.ja.md) | SPA 組み込みパターン |
| [HTML/CSS サイレント印刷](html-css-silent-print.ja.md) | HTML/CSS テンプレート + エージェント |
| [Web からのバッチ・ラベル印刷](batch-label-printing.ja.md) | 配送ラベルとバッチジョブ |
| [リモート / サーバー配信サイレント印刷](remote-silent-print.ja.md) | WMS / サーバーからワークステーションへ |
| [サーマルレシートサイレント印刷](thermal-receipt-silent-print.ja.md) | ブラウザからのレシート印刷 |

## Português (Brasil)

### Comece aqui

| Guia | Sobre |
|---|---|
| [Impressão silenciosa no navegador](browser-silent-print.pt-BR.md) | O que significa “impressão silenciosa” no navegador e por onde continuar |
| [Como funciona a impressão silenciosa](how-silent-printing-works.pt-BR.md) | Visão geral da arquitetura |
| [Como escolher uma stack de impressão silenciosa](choose-silent-print-stack.pt-BR.md) | Árvore de decisão para escolher uma stack |

### Comparações e migrações

| Guia | Sobre |
|---|---|
| [window.print vs impressão silenciosa](window-print-vs-silent-print.pt-BR.md) | Por que `window.print()` sempre abre um diálogo |
| [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.pt-BR.md) | Comparação lado a lado de pontes comuns |
| [Alternativas ao QZ Tray](qz-tray-alternatives.pt-BR.md) | Outras opções se QZ não encaixa |
| [Alternativas ao JSPrintManager](jsprintmanager-alternatives.pt-BR.md) | Outras opções se JSPM não encaixa |
| [Alternativas ao Lodop](lodop-alternatives.pt-BR.md) | Notas de migração saindo de Lodop / C-Lodop |
| [Alternativas ao hiprint](hiprint-alternatives.pt-BR.md) | Opções em torno de stacks estilo hiprint |

### Implementação e ops

| Guia | Sobre |
|---|---|
| [Chrome Local Network Access e 127.0.0.1](chrome-local-network-access.pt-BR.md) | HTTPS + localhost após Chrome LNA |
| [Impressão silenciosa em Vue / React](vue-react-silent-print.pt-BR.md) | Padrões de integração em SPA |
| [Impressão silenciosa com HTML/CSS](html-css-silent-print.pt-BR.md) | Manter templates HTML/CSS com um agente |
| [Impressão em lote e etiquetas a partir da web](batch-label-printing.pt-BR.md) | Etiquetas de envio e jobs em lote |
| [Impressão silenciosa remota / push do servidor](remote-silent-print.pt-BR.md) | WMS / push remoto para estação de trabalho |
| [Impressão silenciosa de cupom térmico](thermal-receipt-silent-print.pt-BR.md) | Impressoras de cupom a partir do navegador |

## Español

### Empieza aquí

| Guía | Acerca de |
|---|---|
| [Impresión silenciosa en el navegador](browser-silent-print.es.md) | Qué significa «impresión silenciosa» en el navegador y dónde continuar |
| [Cómo funciona la impresión silenciosa](how-silent-printing-works.es.md) | Visión general de la arquitectura |
| [Elegir un stack de impresión silenciosa](choose-silent-print-stack.es.md) | Árbol de decisión para elegir un stack |

### Comparaciones y migraciones

| Guía | Acerca de |
|---|---|
| [window.print vs impresión silenciosa](window-print-vs-silent-print.es.md) | Por qué `window.print()` siempre muestra un diálogo |
| [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.es.md) | Comparación lado a lado de puentes habituales |
| [Alternativas a QZ Tray](qz-tray-alternatives.es.md) | Otras opciones si QZ no encaja |
| [Alternativas a JSPrintManager](jsprintmanager-alternatives.es.md) | Otras opciones si JSPM no encaja |
| [Alternativas a Lodop](lodop-alternatives.es.md) | Notas de migración desde Lodop / C-Lodop |
| [Alternativas a hiprint](hiprint-alternatives.es.md) | Opciones alrededor de stacks estilo hiprint |

### Implementación y operaciones

| Guía | Acerca de |
|---|---|
| [Chrome Local Network Access y 127.0.0.1](chrome-local-network-access.es.md) | HTTPS + localhost tras Chrome LNA |
| [Impresión silenciosa en Vue / React](vue-react-silent-print.es.md) | Patrones de integración en SPA |
| [Impresión silenciosa con HTML/CSS](html-css-silent-print.es.md) | Mantener plantillas HTML/CSS con un agente |
| [Impresión por lotes y etiquetas desde la web](batch-label-printing.es.md) | Etiquetas de envío y trabajos por lotes |
| [Impresión silenciosa remota](remote-silent-print.es.md) | WMS / push remoto a estación de trabajo |
| [Impresión silenciosa de recibos térmicos](thermal-receipt-silent-print.es.md) | Impresoras de recibos desde el navegador |

## 한국어

### 여기서 시작

| 가이드 | 내용 |
|---|---|
| [브라우저 사일런트 프린트](browser-silent-print.ko.md) | 브라우저에서 «사일런트 프린트»의 의미와 다음에 읽을 문서 |
| [사일런트 프린트 동작 원리](how-silent-printing-works.ko.md) | 아키텍처 개요 |
| [사일런트 프린트 스택 선택](choose-silent-print-stack.ko.md) | 스택 선택 결정 트리 |

### 비교 및 마이그레이션

| 가이드 | 내용 |
|---|---|
| [window.print vs 사일런트 프린트](window-print-vs-silent-print.ko.md) | `window.print()`가 항상 대화상자를 띄우는 이유 |
| [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.ko.md) | 주요 브리지 나란히 비교 |
| [QZ Tray 대안](qz-tray-alternatives.ko.md) | QZ가 맞지 않을 때 다른 옵션 |
| [JSPrintManager 대안](jsprintmanager-alternatives.ko.md) | JSPM이 맞지 않을 때 다른 옵션 |
| [Lodop 대안](lodop-alternatives.ko.md) | Lodop / C-Lodop에서 마이그레이션 |
| [hiprint 대안](hiprint-alternatives.ko.md) | hiprint 스타일 스택 주변 옵션 |

### 구현 및 운영

| 가이드 | 내용 |
|---|---|
| [Chrome Local Network Access & 127.0.0.1](chrome-local-network-access.ko.md) | Chrome LNA 이후 HTTPS + localhost |
| [Vue / React 사일런트 프린트](vue-react-silent-print.ko.md) | SPA 연동 패턴 |
| [HTML/CSS 사일런트 프린트](html-css-silent-print.ko.md) | HTML/CSS 템플릿 + 에이전트 |
| [웹에서 배치 및 라벨 인쇄](batch-label-printing.ko.md) | 배송 라벨 및 배치 작업 |
| [원격 / 서버 푸시 사일런트 프린트](remote-silent-print.ko.md) | WMS / 서버에서 워크스테이션으로 |
| [열전사 영수증 사일런트 프린트](thermal-receipt-silent-print.ko.md) | 브라우저에서 영수증 프린터 |

## Deutsch

### Hier starten

| Guide | Inhalt |
|---|---|
| [Stille Druckausgabe im Browser](browser-silent-print.de.md) | Was „stille Druckausgabe“ im Browser bedeutet und wohin als Nächstes |
| [Wie stille Druckausgabe funktioniert](how-silent-printing-works.de.md) | Architektur-Überblick |
| [Einen Stack für stille Druckausgabe wählen](choose-silent-print-stack.de.md) | Entscheidungsbaum für die Stack-Wahl |

### Vergleiche & Migrationen

| Guide | Inhalt |
|---|---|
| [window.print vs. stille Druckausgabe](window-print-vs-silent-print.de.md) | Warum `window.print()` immer einen Dialog zeigt |
| [QZ Tray vs. web-print-pdf vs. JSPrintManager](qz-vs-jspm-vs-web-print-pdf.de.md) | Gegenüberstellung gängiger Brücken |
| [QZ Tray-Alternativen](qz-tray-alternatives.de.md) | Andere Optionen, wenn QZ nicht passt |
| [JSPrintManager-Alternativen](jsprintmanager-alternatives.de.md) | Andere Optionen, wenn JSPM nicht passt |
| [Lodop-Alternativen](lodop-alternatives.de.md) | Migrationshinweise weg von Lodop / C-Lodop |
| [hiprint-Alternativen](hiprint-alternatives.de.md) | Optionen rund um hiprint-ähnliche Stacks |

### Implementierung & Betrieb

| Guide | Inhalt |
|---|---|
| [Chrome Local Network Access & 127.0.0.1](chrome-local-network-access.de.md) | HTTPS + localhost nach Chrome LNA |
| [Stille Druckausgabe in Vue / React](vue-react-silent-print.de.md) | SPA-Integrationsmuster |
| [Stille Druckausgabe mit HTML/CSS](html-css-silent-print.de.md) | HTML/CSS-Vorlagen mit Agent behalten |
| [Batch- & Etikettendruck aus dem Web](batch-label-printing.de.md) | Versandetiketten und Batch-Jobs |
| [Remote / serverseitig gesteuerte stille Druckausgabe](remote-silent-print.de.md) | WMS / Server-Push an Arbeitsplätze |
| [Thermobeleg stille Druckausgabe](thermal-receipt-silent-print.de.md) | Belegdrucker aus dem Browser |

## Русский

### Начните здесь

| Гайд | О чём |
|---|---|
| [Тихая печать из браузера](browser-silent-print.ru.md) | Что значит «тихая печать» в браузере и куда идти дальше |
| [Как работает тихая печать](how-silent-printing-works.ru.md) | Обзор архитектуры |
| [Как выбрать стек для тихой печати](choose-silent-print-stack.ru.md) | Дерево решений для выбора стека |

### Сравнения и миграции

| Гайд | О чём |
|---|---|
| [window.print и тихая печать](window-print-vs-silent-print.ru.md) | Почему `window.print()` всегда показывает диалог |
| [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.ru.md) | Сравнение распространённых мостов |
| [Альтернативы QZ Tray](qz-tray-alternatives.ru.md) | Другие варианты, если QZ не подходит |
| [Альтернативы JSPrintManager](jsprintmanager-alternatives.ru.md) | Другие варианты, если JSPM не подходит |
| [Альтернативы Lodop](lodop-alternatives.ru.md) | Заметки по миграции с Lodop / C-Lodop |
| [Альтернативы hiprint](hiprint-alternatives.ru.md) | Варианты вокруг hiprint-подобных стеков |

### Внедрение и эксплуатация

| Гайд | О чём |
|---|---|
| [Chrome Local Network Access и 127.0.0.1](chrome-local-network-access.ru.md) | HTTPS + localhost после Chrome LNA |
| [Тихая печать во Vue / React](vue-react-silent-print.ru.md) | Паттерны интеграции в SPA |
| [Тихая печать с HTML/CSS](html-css-silent-print.ru.md) | HTML/CSS-шаблоны с агентом |
| [Пакетная печать и этикетки из веба](batch-label-printing.ru.md) | Этикетки доставки и пакетные задания |
| [Удалённая / серверная тихая печать](remote-silent-print.ru.md) | WMS / push на рабочую станцию |
| [Тихая печать термочеков](thermal-receipt-silent-print.ru.md) | Чековые принтеры из браузера |

Back to the [main list](../README.md) · [中文列表](../README.zh-CN.md) · [日本語リスト](../README.ja.md) · [Lista en español](../README.es.md) · [Lista em português](../README.pt-BR.md) · [한국어 목록](../README.ko.md) · [Deutsche Liste](../README.de.md) · [Список на русском](../README.ru.md)
