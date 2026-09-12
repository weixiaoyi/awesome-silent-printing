# Альтернативы Lodop

Lodop / C-Lodop по-прежнему распространён во многих Windows-бизнес-системах. Обычно ищут альтернативы, когда нужны:

- Современная интеграция в SPA
- Шире поддержка ОС (macOS / Linux)
- Документация с приоритетом английского для смешанных команд
- Понятнее поведение HTTPS + localhost под текущими правилами Chromium

## Направления замены

| Потребность | Кандидаты |
|---|---|
| Бизнес-документы HTML/CSS | QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, hiprint + electron-hiprint |
| Raw POS / этикетки | [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| Облако на много принтеров | [PrintNode](https://www.printnode.com/en) |
| Остаться на Lodop | Lodop7 / C-Lodop, если стек ещё подходит |

## Почему миграции застревают

- Шаблоны — смесь проприетарных команд Lodop и HTML-фрагментов
- Имена принтеров и лотки зашиты в старых скриптах
- Больницы / ERP боятся менять рабочий путь печати в пик сезона

## Советы по миграции

1. Инвентаризация шаблонов: HTML vs проприетарные команды. Посчитайте «уже только HTML».
2. Пересоберите критичные документы в HTML/CSS, где можно; экзотический raw — второй волной.
3. Старый и новый агент параллельно на пилотном столе (разные порты).
4. Почините HTTPS / Local Network Access до nationwide rollout.
5. Обучите helpdesk новому симптому «агент не запущен» — он заменяет ошибки эпохи ActiveX.

## Связанное

- [Как выбрать стек](choose-silent-print-stack.ru.md)
- [window.print и тихая печать](window-print-vs-silent-print.ru.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ru.md)
