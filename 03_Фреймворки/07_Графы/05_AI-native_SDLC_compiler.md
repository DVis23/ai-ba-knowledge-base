# AI-native SDLC compiler

## Назначение

AI-native SDLC compiler объявляет stable-ID graph долговременным контрактом
поставки. Входом становится человеческое намерение и semantic delta. Выходом —
контекст реализации, план доказательств и release verdict.

```text
human source
  → semantic change
  → approved proposal fingerprint
  → implementation context
  → code + test + runtime evidence
  → readiness gate
  → accepted graph snapshot
```

Слово «compiler» означает, что производные представления не редактируются:
scope, graph slice, worklist, evidence requirements и findings вычисляются из
контракта и наблюдаемых фактов.

## Источник истины

Каноном считается принятый набор stable-ID артефактов и сильных связей.
Markdown остаётся физическим носителем, но его значение определяется положением
в semantic graph.

Пять логических слоёв:

|Слой|Содержание|
|---|---|
|Source|человеческое намерение и происхождение|
|Contract|AC, правила, состояния, flows и ограничения|
|Proof|код, тесты, UI, runtime evidence|
|Change|delta, scope, approval и lifecycle|
|Navigation|слабые упоминания и контейнеры|

Обычное упоминание не повышается до semantic relation. Агент может предложить
`CANDIDATE_TRACE`, но gate игнорирует его до подтверждения.

## Устройство компилятора

```text
human files     canonical artifacts      observed providers
     └───────────────┬──────────────────────────┘
                     ▼
              graph compilation
                     │
       ┌─────────────┼──────────────┐
       ▼             ▼              ▼
 semantic delta   impact paths   evidence state
       │             │              │
       └─────────────┴──────────────┘
                     ▼
       proposal / delivery / release views
                     │
                     ▼
                  verdict
```

### Semantic delta

Git diff преобразуется из списка строк в изменение артефактов и сильных
связей. Manifest-free режим умеет показать delta без отдельного CHG. Полный
workflow добавляет intent, sources, base ref и approval fingerprint.

### Proposal fingerprint

Fingerprint связывает approval с конкретным состоянием:

- canonical artifact delta;
- semantic edge delta;
- конфигурация типов и gates;
- evidence policy;
- base revision.

Изменение любого элемента инвалидирует approval. Механизм предотвращает
подмену согласованного предложения после review.

### Bounded context

Компилятор строит контекст из scope, сильных соседей и impact paths. Слабые
`MENTIONS` по умолчанию исключаются. Агент получает связанную подзадачу вместо
полного репозитория.

### Вычисляемое состояние

|Флаг|Основание|
|---|---|
|`implemented`|есть допустимый implementation path|
|`verified`|есть test/evidence path|
|`changing`|узел входит в активный change|
|`stale`|fingerprint разошёлся с принятым снимком|

Поздняя версия различает `COVERS`, `EXECUTES` и `VERIFIES`. Тестовая декларация
не доказывает запуск. Runtime evidence хранит статус, время, environment
fingerprint и профиль выполнения.

### Execution profiles

Deterministic и live проверки независимы:

```text
deterministic: fixtures, mocks, controlled runtime
live:          реальный sandbox или provider
```

PASS одного профиля не заменяет другой, если policy требует оба.

### Durable references

Номер строки — нестабильный адрес. Зрелая реализация хранит `anchor_text` и
`ref_id`. Review и validate находят определение по уникальному содержимому, а
line number используют как подсказку.

### Framework adapters

Универсальное ядро принимает provider facts. `mini-graph-ba` экспортирует
registry resources, CustomMethod, admin pages, integrations, React components
и runtime topology. Semantic projection отбрасывает framework boilerplate и
оставляет продуктовые связи.

## Gate как арбитр

В этой модели gate имеет процессную власть. Release verdict зависит от:

- принятого proposal fingerprint;
- реализованности контрактных узлов;
- требуемого evidence;
- отсутствия blocking gaps;
- отсутствия drift или stale proof.

Это сильнее структурной диагностики. Граф не просто сообщает наблюдение, а
решает, допустим ли переход lifecycle.

## Главный риск

Структурная полнота не равна продуктовой корректности. Неправильный AC может
иметь идеальные связи, код и зелёные тесты. Fingerprint доказывает неизменность
согласованного текста, но не заменяет человеческое понимание текста.

Второй риск — дублирование владельца изменений. Если рядом существует OpenSpec
с proposal, tasks, review и archive, graph-native workflow создаёт второй scope,
approval и verdict.

## Результирующая версия

`0.3.0` ввела capability-driven SDLC graph. `0.3.1` завершила readiness loop и
runtime evidence. После переноса в mini ядро стало `0.4.0`: появились зрелые
provider contracts, evidence profiles, durable refs и semantic projections.

`0.4.0` — результирующая техническая версия этой линии. Проект может применять
её в другой роли, потому что назначение задаётся политикой, а не номером пакета.

Следующая модель: [trace-only semantic projection](06_Trace-only_semantic_projection.md).
