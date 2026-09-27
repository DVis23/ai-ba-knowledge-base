# OpenSpec

**Статус:** описание конкретной SDD-системы.

OpenSpec хранит согласованное поведение системы и оформляет предлагаемые
изменения отдельными пакетами. Его ключевая идея — не один большой план, а
набор связанных артефактов, которые можно уточнять по мере получения знаний.

## Навигация

- [Specs, changes и capability](01_Specs_changes_и_capability.md)
- [Артефакты и schema workflow](02_Артефакты_и_schema.md)
- [Requirements, scenarios и delta specs](03_Requirements_scenarios_и_deltas.md)
- [OPSX lifecycle](04_OPSX_lifecycle.md)
- [Источники и трассировка](05_Источники_и_трассировка.md)
- [Frontend, backend и UI-источники](06_Frontend_backend_и_UI.md)
- [Design, tasks и валидация](07_Design_tasks_и_валидация.md)
- [Настройка OpenSpec](08_Настройка_OpenSpec.md)
- [Границы применения](09_Границы_применения.md)

## Что чем владеет

| Сущность | Полномочие |
|---|---|
| `openspec/specs/` | Принятое текущее поведение |
| `openspec/changes/<name>/` | Предлагаемое изменение и план его реализации |
| `proposal.md` | Намерение, scope и общий подход change |
| Delta specs | Изменение поведения относительно living specs |
| `design.md` | Технические решения change |
| `tasks.md` | Выполнимая декомпозиция реализации |
| Код | Наблюдаемое текущее состояние реализации |
| Результаты проверок | Доказательство фактического поведения |

OpenSpec не владеет человеческими первоисточниками и не доказывает реализацию.
Он связывает утверждённое намерение с технической работой.

## Источники по системе

- [Concepts](https://github.com/Fission-AI/OpenSpec/blob/main/docs/concepts.md)
- [Writing Good Specs](https://github.com/Fission-AI/OpenSpec/blob/main/docs/writing-specs.md)
- [OPSX](https://github.com/Fission-AI/OpenSpec/blob/main/docs/opsx.md)
- [Customization](https://github.com/Fission-AI/OpenSpec/blob/main/docs/customization.md)
