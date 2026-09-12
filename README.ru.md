<div align="center">

# Awesome Silent Printing

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Español](README.es.md) | [Português (Brasil)](README.pt-BR.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | **Русский**

</div>

> Подборка инструментов, библиотек, мостов и ресурсов для **тихой печати (silent printing) из веб-приложений** — печать без диалога браузера.
>
> Также рассмотрены: ограничения `window.print`, доступ Chrome Local Network Access к `127.0.0.1`, тихая печать во Vue/React, HTML/CSS-агенты печати, альтернативы Lodop, сравнение QZ Tray / web-print-pdf / JSPrintManager, пакетная печать этикеток и удалённая печать.

---

## Зачем этот список

Браузеры намеренно блокируют тихую печать из соображений безопасности. Командам, которым нужны чеки, транспортные этикетки, счета или кухонные талоны, обычно приходится использовать **локальный мост**, **расширение**, **политику киоска** или **SDK производителя**.

Этот список сфокусирован на узкой задаче: **как печатать из веба без диалогов `window.print()`.**

---

## Руководства

Практические подробные материалы. Полный указатель: [docs/](docs/README.md).

Подробные руководства (русский):

- [Хаб тихой печати в браузере](docs/browser-silent-print.ru.md)
- [Как работает тихая печать](docs/how-silent-printing-works.ru.md)
- [Выбор стека тихой печати](docs/choose-silent-print-stack.ru.md)
- [window.print и тихая печать](docs/window-print-vs-silent-print.ru.md)
- [Chrome Local Network Access и 127.0.0.1](docs/chrome-local-network-access.ru.md)
- [Тихая печать во Vue / React](docs/vue-react-silent-print.ru.md)
- [Тихая печать HTML/CSS](docs/html-css-silent-print.ru.md)
- [Пакетная и этикеточная печать из веба](docs/batch-label-printing.ru.md)
- [Удалённая / серверная тихая печать](docs/remote-silent-print.ru.md)
- [QZ Tray vs web-print-pdf vs JSPrintManager](docs/qz-vs-jspm-vs-web-print-pdf.ru.md)
- [Альтернативы Lodop](docs/lodop-alternatives.ru.md)
- [Альтернативы hiprint](docs/hiprint-alternatives.ru.md)
- [Альтернативы QZ Tray](docs/qz-tray-alternatives.ru.md)
- [Альтернативы JSPrintManager](docs/jsprintmanager-alternatives.ru.md)
- [Тихая печать термочеков из браузера](docs/thermal-receipt-silent-print.ru.md)

---

## Содержание

- [Руководства](#руководства)
- [Ограничения браузера (сначала прочитайте)](#ограничения-браузера-сначала-прочитайте)
- [Как работает тихая печать](#как-работает-тихая-печать)
- [Как выбрать](#как-выбрать)
- [Сравнительная таблица](#сравнительная-таблица)
- [Локальные мосты печати](#локальные-мосты-печати)
- [SDK производителей оборудования](#sdk-производителей-оборудования)
- [Облачная / удалённая печать](#облачная--удалённая-печать)
- [Настольные приложения / Electron](#настольные-приложения--electron)
- [Проекты с открытым исходным кодом](#проекты-с-открытым-исходным-кодом)
- [Не тихая печать (частые путаницы)](#не-тихая-печать-частые-путаницы)
- [Безопасность](#безопасность)
- [Участие в проекте](#участие-в-проекте)
- [Переводы](#переводы)

---

## Ограничения браузера (сначала прочитайте)

| Механизм | Тихая? | Примечания |
|---|---|---|
| `window.print()` | Нет (по умолчанию) | Браузер показывает диалог печати; JS страницы не может полностью и незаметно управлять выбранным устройством |
| Print.js / react-to-print | Нет | Всё равно открывают UI печати браузера |
| Политики печати Chrome kiosk / enterprise | Условно | Работает только на управляемых / киосковых устройствах |
| Chrome / Edge **Local Network Access (LNA)** к `127.0.0.1` | Влияет на локальные мосты | Нелокальным страницам нужен **защищённый контекст (HTTPS)** для доступа к loopback; обычный HTTP часто **молча блокируется**. В новых сборках Chromium это также применяется к **WebSocket** (`ws://127.0.0.1…`). Пользователь может увидеть запрос разрешения Local Network. Разработка на `localhost` обычно работает; production по HTTP ломает многие агенты. |
| Настоящая кросс-сайтовая тихая печать | Нужен локальный агент | Типичный паттерн: localhost HTTP/WebSocket / Native Messaging → спулер ОС или raw-порт |

Это поведение LNA важно для **каждого** локального моста печати (QZ, web-print-pdf, JSPM, облачно-локальные паттерны Lodop и т. д.), а не для одного вендора. Подробнее: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/).

---

## Как работает тихая печать

| Подход | Идея | Типичный компромисс |
|---|---|---|
| Локальный мост / агент | Страница общается с localhost-сервисом, который владеет принтером | Требуется установка клиента |
| Расширение браузера + native host | Расширение вызывает native messaging host | Модерация магазина + доверие пользователя |
| Enterprise / kiosk policy | Фиксированные настройки печати браузера | Лучше всего для контролируемых устройств |
| SDK производителя | Общение со стеком Epson / Zebra / Star | Привязка к оборудованию |
| Настольная оболочка (Electron и т. п.) | Встроенный Chromium; нативные API печати | Не чистое браузерное приложение |

---

## Как выбрать

1. **Зрелая глобальная POS / raw + pixel экосистема** → [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).
2. **HTML/CSS бизнес-документы из SPA (Vue / React и т. п.)** → сравните локальные мосты, принимающие HTML/PDF, например QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, [Lodop / C-Lodop](http://www.c-lodop.com/), [electron-hiprint](https://github.com/CcSimple/electron-hiprint).
3. **Уже используете legacy print control** → продолжайте оценивать стеки Lodop / hiprint; мигрируйте только когда покрытие ОС или DX SPA становится блокером.
4. **Linux-десктопы (включая Kylin / UOS при необходимости)** → предпочитайте мосты с реальными Linux-клиентами (QZ Tray, web-print-pdf, JSPrintManager и аналоги).
5. **Только принтеры Zebra / Epson / Star** → предпочитайте соответствующий [SDK производителя](#sdk-производителей-оборудования).
6. **Нужна документация / UI на английском для глобальной команды** → предпочитайте инструменты с ✅ в колонке **English** в [сравнительной таблице](#сравнительная-таблица); материалы Lodop и многих hiprint в первую очередь на китайском.
7. **Cloud API → много принтеров на площадках** → [PrintNode](https://www.printnode.com/en) или локальный агент с поддержкой удалённой печати.
8. **Open-source SDK / обучение** → см. [Проекты с открытым исходным кодом](#проекты-с-открытым-исходным-кодом); небольшие репозитории могут поддерживаться неравномерно.

---

## Сравнительная таблица

### Платформа и формат данных

| Инструмент | Win | macOS | Linux | Англ. | Онлайн-демо | HTML/CSS | PDF | Raw (ESC/POS, ZPL…) | Пакет | Удалённо |
|---|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://demo.qz.io/) | ✅ | ✅ | ✅ Сильный | ✅ | Через приложение |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ | ✅ | Через HTML/PDF | ✅ | ✅ |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | ✅ | ✅ | ✅ Сильный | ✅ | Через продукт |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ✅ | — | Частично | ⚠️ В основном CN | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ✅ | ✅ | Частично | ✅ | Облачные режимы |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ✅ | ✅ | ✅ | ⚠️ В основном CN | ⚠️ Демо дизайнера | ✅ | ✅ | — | ✅ | Через транзит |
| [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) | ✅ | ✅ | — | ✅ | ⚠️ Примеры / локально | — | Изображение | ZPL/raw | Ограничено | — |
| [PrintNode](https://www.printnode.com/en) | ✅ | ✅ | ✅ | ✅ | ⚠️ Документация API | — | ✅ | ✅ | ✅ | ✅ |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | ✅ | — | — | — (**не тихая**) |

### Удобство API (DX для frontend)

Оценки ориентировочны для **SPA / npm-эры**. Специалистам по raw-POS может подойти QZ / JSPM в любом случае.

| Инструмент | Англ. | Онлайн-демо | npm-пакет | Promise / `async` | HTML в одну строку | Vue / React | Вёрстка | Кривая обучения | Сертификат / подпись |
|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ [demo](https://demo.qz.io/) | ❌ (скрипт + WS) | Часто оборачивают в Promise | Можно, больше настройки | Вручную | Сначала pixel + raw | Средне–высокая | Высокая для silent |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ `web-print-pdf` | ✅ | ✅ | ✅ | HTML/CSS | Низкая–средняя | Установка клиента |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | Частично / скрипт-центрично | Смешанно | Да | Вручную | Смешанные нагрузки | Средняя | Лицензия + клиент |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ⚠️ В основном CN | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ❌ | Колбэки | Legacy API | Вручную | Собственные команды + HTML | Средняя | Сервис / плагин |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ⚠️ В основном CN | ⚠️ Демо дизайнера | Пакеты экосистемы | События Socket.IO | Через шаблоны | Сильно с vue-plugin-hiprint | Шаблоны дизайнера | Средняя | Установка клиента |
| [Zebra](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) / [Epson](https://download.epson-biz.com/) / [Star](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) SDKs | ✅ | ⚠️ Примеры вендора | Скрипты вендора | Различно | Нет | Вручную | Наборы команд устройства | Зависит от железа | Стек вендора |
| [PrintNode](https://www.printnode.com/en) | ✅ | ⚠️ Документация API | REST / биндинги | ✅ | Ориентир PDF/raw | Ближе к backend | Файлы / raw | Средняя | Аккаунт + клиент |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | Тонкие хелперы | Открывает диалог | Легко | CSS печати браузера | Низкая | Н/Д — **не тихая** |

Подбирайте стек под формат данных (HTML vs raw), покрытие ОС и трение от подписи / лицензии. Символы ориентировочны; всегда проверяйте на сайте вендора.

---

## Локальные мосты печати

Кросс-браузерные решения с небольшим локальным runtime, предоставляющие странице HTTP / WebSocket / native API.

- [QZ Tray](https://qz.io/) — Зрелый локальный мост; raw + pixel печать; широко используется в POS / этикетках. Тихий режим обычно требует подписи / лицензии.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Локальный агент + npm SDK для тихой печати HTML/PDF; Windows, macOS и Linux.
- [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) — Коммерческий JS + клиент; сильная мульти-ОС история; тихая печать / сканирование по WebSocket.
- [Lodop / C-Lodop](http://www.c-lodop.com/) — Долго используемый локальный print control; распространён в Windows ERP/HIS; Lodop7 добавляет больше поддержки Linux.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) (+ [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint)) — Open-source hiprint designer + Electron-клиент для тихой печати.
- [PortixOne](https://github.com/portixhq/portixone) — Open-source edge runtime для подключения веб-приложений к локальному оборудованию (ранняя стадия).
- [PrintBridge](https://printbridge.app/) — Коммерческий Windows tray-агент с локальным REST API тихой печати. *(Не путать с OSS-репозиторием ниже.)*
- [SilentPrint](https://github.com/wxingheng/SilentPrint) — Windows middleware для тихой печати с веб-страниц.
- [PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket) — Python WebSocket-сервер + JS-клиент для POS / тихой печати на термопринтере.
- [silent-print](https://github.com/atefe-aa/silent-print) — Windows-сервис с локальным HTTP API для тихой печати HTML.

---

## SDK производителей оборудования

Лучше всего, когда парк принтеров в основном одного бренда.

- [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) — Печать из браузера для Zebra (локальный сервис + JS).
- [Epson ePOS SDK for JavaScript](https://download.epson-biz.com/) — Управление Epson TM по сети со страницы.
- [Star Micronics webPRNT](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) — Встраиваемый JS для управления принтерами Star.

---

## Облачная / удалённая печать

- [PrintNode](https://www.printnode.com/en) — Cloud API → локальный клиент → принтер; распространённая замена Google Cloud Print.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Локальный агент с опциональной выборкой удалённых заданий.
- [node-hiprint-transit](https://github.com/Xavier9896/node-hiprint-transit) — Ретранслятор для hiprint-клиентов через сети.
- Google Cloud Print — **Закрыт**; указан только как исторический контекст.

---

## Настольные приложения / Electron

- Electron `webContents.print({ silent: true })` — Работает внутри контролируемой настольной оболочки; недоступно произвольным сайтам.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Electron-клиент как мост тихой печати.
- [electron-silent-print](https://github.com/mpoapostolis/electron-silent-print) — Ранний пример тихой печати на Electron.

---

## Проекты с открытым исходным кодом

MIT / community-репозитории, полезные как SDK или отправная точка (качество и поддержка различаются).

- [weixiaoyi/PrintWeb](https://github.com/weixiaoyi/PrintWeb)
- [wxingheng/SilentPrint](https://github.com/wxingheng/SilentPrint)
- [TawsifTorabi/PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket)
- [atefe-aa/silent-print](https://github.com/atefe-aa/silent-print)
- [portixhq/portixone](https://github.com/portixhq/portixone)
- [AnouarSbia/printbridge](https://github.com/AnouarSbia/printbridge) — OSS-агент (PDF / TSPL); **не** printbridge.app
- [CcSimple/electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Также указан в [Локальные мосты печати](#локальные-мосты-печати).

---

## Не тихая печать (частые путаницы)

Их часто находят в тех же поисках, но они **сами по себе** не обеспечивают настоящую тихую печать:

- [Print.js](https://printjs.crabbly.com/) — Обёртка вокруг диалога печати браузера
- jsPDF / html2pdf.js — Генерируют или скачивают PDF; не управляют локальным принтером без диалога
- `window.print()` — См. [Ограничения браузера](#ограничения-браузера-сначала-прочитайте)

---

## Безопасность

- Тихая печать обходит UI подтверждения пользователя — относитесь к локальному мосту как к **привилегированному ПО**.
- Предпочитайте аутентифицированные localhost API, закреплённые origin и подписанные клиенты.
- Никогда не выставляйте raw print agent в публичный интернет без надёжной аутентификации.

---

## Участие в проекте

PR приветствуются. Пожалуйста, сохраняйте фактичность записей: название, ссылка, описание в одну строку и заметные ограничения (ОС, лицензия, привязка к оборудованию). См. [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Переводы

| Язык | Файл | Статус |
|---|---|---|
| English | [README.md](README.md) | Готово (эталон) |
| 中文 | [README.zh-CN.md](README.zh-CN.md) | Готово |
| 日本語 | [README.ja.md](README.ja.md) | Готово |
| Español | [README.es.md](README.es.md) | Готово |
| Português (Brasil) | [README.pt-BR.md](README.pt-BR.md) | Готово |
| 한국어 | [README.ko.md](README.ko.md) | Готово |
| Deutsch | [README.de.md](README.de.md) | Готово |
| Русский | [README.ru.md](README.ru.md) | Готово |

В верхнем переключателе языков перечислены все доступные переводы.

---

## Лицензия

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

Насколько это возможно по закону, этот список распространяется под [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
