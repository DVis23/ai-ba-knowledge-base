# Trace-only semantic projection

## Назначение

Trace-only semantic projection использует зрелый движок graph-ba как
read-oriented индекс. Канонический SDD и lifecycle находятся во внешнем
владельце — в UpSushi это OpenSpec.

Разделение полномочий:

```text
OpenSpec
  requirements · proposal · review · tasks · apply · acceptance · archive

graph-ba
  search · node · path · impact · coverage · code refs · trace gaps
```

Граф может вычислить структурное состояние, но его verdict не является
разрешением на поставку.

## Источники и авторитет

|Класс|Роль|
|---|---|
|Human sources|RAC, надиктовки, решения владельца, дизайнерские наблюдения|
|Canonical SDD|одобренные delta specs и living OpenSpec specs|
|Historical context|старые BA-артефакты и `.graphba/contract`|
|Implementation|код, registry, services, UI|
|Evidence|тестовые трассы и зафиксированные запуски|

Исторический stable-ID узел может быть полезен для поиска. Его присутствие в
графе не делает его актуальным требованием.

Code и tests подтверждают текущее состояние, но не создают продуктовый смысл.
Если между реализацией и OpenSpec есть конфликт, исправляется реализация или
поднимается вопрос. Граф не выбирает победителя.

## Устройство

```text
human RAC ───────────┐
approved OpenSpec ───┤
historical BA ───────┼─→ graph-ba import ─→ SQLite/FTS graph
code @trace ─────────┤                         │
test inventory ──────┤                         ├─ search/path/impact
observed providers ──┘                         └─ coverage/trace gaps
```

### Canonical path

Работа начинается от принятого `AC-*`, затем исследуются входящие и исходящие
связи:

```text
AC ─NORMALIZES→ RAC
CODE ─IMPLEMENTS→ AC
TEST ─COVERS→ AC
EVD ─EXECUTES→ TEST
EVD ─VERIFIES→ AC
```

Направление `AC → RAC` выражает нормализацию человеческого входа в канонический
критерий. Авторитет определяется provenance и review state, а не направлением
стрелки.

### Observed projection

Framework adapter строит производные узлы реализации. Для Mini это:

- registry resources и declarations;
- CustomMethod и операции;
- admin pages, actions и UI zones;
- React components;
- integration connections и triggers;
- test inventory.

Semantic profile удаляет каркас framework: сгенерированные адаптеры, системные
роли и служебную топологию без продуктового смысла. Полный профиль остаётся для
диагностики.

### Производные файлы

Observed YAML, SQLite и отчёты генерируются. Их нельзя редактировать вручную.
Перед доверием к code/test trace обновляется provider projection.

### Evidence boundary

`COVERS` означает намерение теста. Выполнение подтверждается только реальным
test run или evidence record. Green coverage показывает связанность, а не факт
успешного выполнения.

### Human boundary

Агент может:

- найти источник;
- показать путь RAC → AC → CODE/TEST;
- обнаружить dangling ID или trace gap;
- оценить технический blast radius.

Агент не может вывести человеческое одобрение из связности графа, зелёного
теста или отсутствия gaps.

## Запрещённые использования

В trace-only модели не применяются как активный delivery workflow:

- `graph-ba change propose/approve/show`;
- graph-native tasks;
- `gate` как acceptance verdict;
- архивирование фичи по состоянию графа;
- старые `.graphba/changes` как план новой работы.

Команды могут оставаться в CLI ради совместимости. Политическая доступность
команды не следует из её технического существования.

## Проверка готовности вне графа

Готовность устанавливает владеющий SDD flow:

1. требования одобрены человеком;
2. открытые продуктовые вопросы закрыты;
3. реализация соответствует in-scope AC;
4. выбранные проверки реально выполнены;
5. tasks закрыты;
6. strict validation проходит;
7. реализация принята человеком там, где это требуется.

graph-ba предоставляет доказательные ссылки для этих решений, но не принимает
решение сам.

## Версия и статус

Эта модель не получила отдельный номер ядра. UpSushi использует `graph-ba 0.4.0`
из снимка mini `0.6.4` и `mini-graph-ba 0.6.4`. Смена произошла в
проектном контракте сентября 2026 года.

Это возврат к исходной границе БА-графа на более зрелом устройстве:

```text
раньше: граф индексирует BA-документы
сейчас: граф индексирует весь путь от human source до runtime evidence

в обоих случаях: граф не является продуктовой истиной
```
