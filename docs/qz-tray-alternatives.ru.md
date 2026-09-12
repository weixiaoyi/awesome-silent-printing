# Альтернативы QZ Tray

Поиск **альтернативы QZ Tray** обычно означает: нужна тихая печать из браузера, но с другим компромиссом по стилю API, цене, подписи или HTML/CSS workflow.

## Когда остаться на QZ Tray

- Raw ESC/POS / ZPL — основная нагрузка
- Уже вложились в подпись и сертификаты QZ
- Нужна длинная глобальная история в POS
- Команда уже оборачивает QZ WebSocket в проде

## Когда оценить альтернативы

| Потребность | Посмотреть |
|---|---|
| Коммерческий JS-клиент с широкими типами файлов | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| npm-стиль HTML/CSS из SPA | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) и другие HTML-capable мосты |
| Cloud API на много принтеров | [PrintNode](https://www.printnode.com/en) |
| Шаблоны через designer | hiprint + electron-hiprint |
| Один бренд железа | SDK Zebra / Epson / Star |

## Заметки по миграции

1. Инвентаризация: какие задания raw, какие HTML/PDF — raw дороже переписывать.
2. Перепроверьте допущения про подпись / лицензию; у следующего vendor не факт «unsigned silent».
3. Один стол с QZ оставьте живым, пока пилотируете альтернативу.
4. Повторно проверьте Chrome Local Network Access на новом порту агента.

## Сравнить напрямую

- [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.ru.md)
- [Как выбрать стек для тихой печати](choose-silent-print-stack.ru.md)
