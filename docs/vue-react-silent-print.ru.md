# Тихая печать во Vue / React

## Цель

Вызывать тихую печать из SPA без `window.print()`.

## Типичная интеграция

1. Пользователь один раз устанавливает локальный агент печати.
2. Frontend подключает npm SDK или JS производителя (QZ, web-print-pdf, JSPM и т. д.).
3. Страница отправляет HTML, PDF или raw на `127.0.0.1`.
4. Агент рендерит / передаёт в принтер ОС.

Форма вызова (API различаются):

```js
// Псевдокод — смотрите документацию вашего моста
await printAgent.printHtml(
  '<div class="label">Order #1001</div>',
  { printer: 'LabelPrinter' }
);
```

### Рекомендуемая граница модуля

Спрячьте печать за небольшим сервисом, чтобы компоненты Vue/React оставались простыми:

```js
// printService.js
export async function printLabel(html, printer) {
  await ensureAgent();
  return printAgent.printHtml(html, { printer });
}
```

- Вызывайте `ensureAgent()` при старте приложения или перед первым экраном печати.
- Показывайте инструкцию по установке, когда агента нет.
- Не вызывайте печать из десяти компонентов с десятью разными объектами опций.

## Подводные камни SPA

| Камень | Решение |
|---|---|
| Печать до поднятия агента | Preflight-подключение / подсказка по установке |
| Жёстко зашитые имена принтеров | Запрос списка; сохранение на станцию |
| HTTP origin в проде | Перейти на HTTPS для Local Network Access |
| Стили не как на экране | Предпочитать Chromium HTML→print агенты; закрепить шрифты |
| Печать живого дерева Vue/React с UI-хромом | Отдельный print root / offscreen-шаблон |
| Огромные base64-картинки inline | URL, которые агент может загрузить, или сжатие |
| Игнор rejections Promise | Ошибки в toast + retry; логировать job id |

## Заметки по Vue

- Шаблоны — в отдельном SFC только для печати (`LabelTicket.vue`), не в полной вёрстке страницы.
- Предпочитайте `ref` + `innerHTML` / `outerHTML` смонтированного print root или HTML-строки из данных.
- Не печатайте, пока `<Transition>` или virtual list в середине обновления.

## Заметки по React

- Та же идея: компонент `PrintTicket` в скрытом контейнере, затем сериализация HTML.
- Осторожно с portals и concurrent rendering — снимок, когда данные стабильны.
- Не полагайтесь на `window.print()` в `useEffect` как «временный» тихий путь — это закрепляет плохую привычку.

## Жизненный цикл подключения

```text
Старт приложения
  → ping агента
  → если down: баннер + ссылка на установку
  → если up: кэш списка принтеров
Пользователь нажимает Печать
  → повторный ping (дёшево)
  → отправка задания
  → результат / код ошибки
```

## Связанное

- [Тихая печать с HTML/CSS](html-css-silent-print.ru.md)
- [Chrome Local Network Access](chrome-local-network-access.ru.md)
- [Как выбрать стек](choose-silent-print-stack.ru.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ru.md)
