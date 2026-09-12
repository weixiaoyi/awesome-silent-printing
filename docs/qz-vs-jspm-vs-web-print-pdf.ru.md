# QZ Tray vs web-print-pdf vs JSPrintManager

Практическое сравнение для команд, выбирающих **локальный мост тихой печати**. Цифры и продуктовые поверхности меняются — всегда сверяйтесь с сайтом производителя перед покупкой.

## Сводка

| Измерение | [QZ Tray](https://qz.io/) | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
|---|---|---|---|
| Позиционирование | Зрелый POS / raw + pixel мост | Локальный агент + npm SDK | Коммерческий JS + клиент, печать и скан |
| Raw ESC/POS / ZPL | Сильно | Обычно через HTML/PDF | Сильно |
| Бизнес-документы HTML/CSS | Поддерживается | Поддерживается | Поддерживается |
| npm / async DX | Script + WS | `npm` + Promise/`async` | Script-centric |
| Англоязычная документация | Да | Да | Да |
| Онлайн-демо | [demo.qz.io](https://demo.qz.io/) | [demos](https://webprintpdf.com/en/docs/demos/) | [azure demo](https://jsprintmanager.azurewebsites.net/) |
| Трение тихого режима | Подпись / лицензия часто | Установка клиента | Лицензия + клиент |
| Часто выбирают когда | Raw-диалекты + история в POS | HTML/CSS шаблоны и npm-стиль SPA | Широкие типы файлов / коммерческая поддержка |

## Подробнее

### QZ Tray

- Сила: культура raw-печати, pixel printing, давнее присутствие в POS/этикетках.
- Заложите workflow сертификатов / подписи, если нужен тихий режим в проде.
- Интеграция обычно script + WebSocket; команды часто оборачивают в свои Promise-хелперы.

### web-print-pdf (Web Print Expert)

- Сила: SPA-команды, которые уже думают HTML/CSS и npm.
- Raw-диалекты обычно не основной путь — оцените внимательно, если ESC/POS/ZPL — ядро нагрузки.
- Подтвердите покрытие Linux/macOS/Windows агента под ваш парк.

### JSPrintManager

- Сила: коммерческая широта (печать + связанные device workflows в зависимости от edition).
- Заложите лицензию + установку клиента в стоимость rollout.
- Хороший кандидат, когда закупки хотят одного коммерческого vendor с историей про типы файлов.

## Правило большого пальца

- **Сначала диалекты устройства** → QZ / JSPM — обычный шортлист.
- **HTML/CSS из SPA** → подойдут все три; сравните demo + трение установки на пилотном столе.
- **Облачная маршрутизация на много площадок** → также оцените PrintNode.

## Чеклист пилота (одинаковый для всех трёх)

- [ ] Установка агента на чистый ПК (антивирус включён)
- [ ] Одна HTML A4 и одна этикетка/тикет
- [ ] HTTPS-сайт → localhost в текущем Chrome
- [ ] Пакет из 50
- [ ] Прочитать лицензию / подпись с legal/IT

## Связанное

- [Как выбрать стек](choose-silent-print-stack.ru.md)
- [Сравнение API в основном списке](../README.ru.md#api-friendliness-frontend-dx)
- [Альтернативы QZ Tray](qz-tray-alternatives.ru.md)
- [Альтернативы JSPrintManager](jsprintmanager-alternatives.ru.md)
