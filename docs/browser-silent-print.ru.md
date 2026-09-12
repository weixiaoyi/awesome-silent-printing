# Тихая печать из браузера

**Тихая печать из браузера** — это когда веб-страница отправляет задание на локальный принтер **без** диалога печати браузера.

Здесь собраны типичные сценарии, с которыми сталкиваются на практике:

- тихая печать из веб-приложения
- печать без диалога в Chrome
- тихая печать во Vue / React
- альтернативы `window.print()`

## Краткий ответ

Обычный сайт сам по себе не может незаметно управлять произвольным принтером. Нужен **локальный агент**, **SDK производителя**, **политика киоска** или **десктопная оболочка**.

Если кто-то говорит о «чистом JavaScript для тихой печати в Chrome на любом принтере», спросите, какой локальный компонент они устанавливают. Именно он и является реальным путём к принтеру.

## Модель за минуту

```text
Страница (HTTPS)
  → мост localhost
  → спулер ОС или raw-порт
  → физический принтер
```

Подробнее: [Как работает тихая печать](how-silent-printing-works.ru.md).

## Куда перейти дальше

| Ваша ситуация | Читать |
|---|---|
| Нужна архитектура | [Как работает тихая печать](how-silent-printing-works.ru.md) |
| Нужно выбрать стек | [Как выбрать стек для тихой печати](choose-silent-print-stack.ru.md) |
| Приходите с `window.print` | [window.print и тихая печать](window-print-vs-silent-print.ru.md) |
| В проде не достучаться до `127.0.0.1` | [Chrome Local Network Access](chrome-local-network-access.ru.md) |
| Интеграция в SPA | [Тихая печать во Vue / React](vue-react-silent-print.ru.md) |
| HTML-шаблоны | [Тихая печать с HTML/CSS](html-css-silent-print.ru.md) |
| Этикетки / складской объём | [Пакетная печать и этикетки](batch-label-printing.ru.md) |
| WMS отправляет задания на рабочие места | [Удалённая тихая печать](remote-silent-print.ru.md) |
| Чеки / кухонные тикеты | [Тихая печать на термопринтере](thermal-receipt-silent-print.ru.md) |
| Сравнение основных мостов | [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ru.md) |

## Частые запросы → гайд

| Люди ищут | Начать здесь |
|---|---|
| browser silent print / webpage silent print | Эта страница |
| window.print without dialog | [window.print и тихая печать](window-print-vs-silent-print.ru.md) |
| Chrome websocket 127.0.0.1 failed | [Chrome LNA](chrome-local-network-access.ru.md) |
| Lodop / hiprint / QZ alternative | [Lodop](lodop-alternatives.ru.md) · [hiprint](hiprint-alternatives.ru.md) · [QZ](qz-tray-alternatives.ru.md) · [JSPM](jsprintmanager-alternatives.ru.md) |

## Список инструментов

Смотрите подборку: [Awesome Silent Printing](../README.ru.md).
