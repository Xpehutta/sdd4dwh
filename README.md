# sdd4dwh — пример реального SDD-воркфлоу для DWH (OpenSpec)

Демонстрация spec-driven development вокруг существующего SQL-файла: от «в репозитории один файл» до цикла изменений с живой спецификацией.

**Старт:** один файл — `sql/d_agr_cred_load.sql`: загрузка DDS-таблицы `s_grnplm_vd_t_bvd_db_dmslcl.d_agr_cred` (349 строк, три потока `UNION ALL`).
**Задача:** внести изменения, предложенные по результатам анализа, — сделать загрузку детерминированной (дата отчёта вместо `now()`).
**Инструмент:** [OpenSpec](https://openspec.dev) — spec-driven, brownfield-first. Артефакты — на русском (`language: ru`); заголовки и SHALL/WHEN/THEN — на английском.

## История (git-теги)

| Тег | Событие |
|---|---|
| `01-baseline` | в репозитории только SQL-файл |
| `02-change1-proposal` | `openspec init` + первый change `add-d-agr-cred-load`: спецификация затронутого среза (proposal → specs → design → tasks) |
| `03-change1-archived` | change заархивирован → появилась живая спека `openspec/specs/credit-agreements/spec.md` |
| `04-change2-proposal` | change `make-load-deterministic`: delta MODIFIED + ADDED — детерминированность по `report_dt` |
| `05-change2-applied` | SQL изменён: `('now')::date` → `:'report_dt'::date` + комментарии |
| `06-change2-archived` | change заархивирован → спека обновлена |
| `07-docs` | этот README |

Как смотреть:

```bash
git diff 04-change2-proposal 05-change2-applied   # код-дифф изменения
git diff 05-change2-applied 06-change2-archived   # что делает archive со спекой
git diff 01-baseline 06-change2-archived          # что SDD добавил вокруг кода целиком
```

## Что демонстрирует

- **Brownfield-first**: спеки растут из изменений; ничего не «бэкфиллим» заранее.
- **Delta (ADDED / MODIFIED)**: изменение описывается относительно текущего поведения; `archive` вливает дельту в живой спек, а сам change уезжает в журнал `changes/archive/`.
- **Ревью по спеке до кода**: proposal → specs → design → tasks → apply.
- **Валидация ловит ошибки**: `openspec validate --strict` не даёт, например, MODIFIED-блоку молча «потерять» существующий сценарий (одна такая ошибка была поймана и исправлена в этом прогоне).
- **Сценарии = тесты**: WHEN/THEN из спеки становятся проверками (регламент загрузки, классификация клиента).

## Структура

```
sql/d_agr_cred_load.sql                      # загрузка (psql-параметр :'report_dt')
openspec/config.yaml                         # язык ru + контекст проекта
openspec/specs/credit-agreements/spec.md     # живая спека (после archive)
openspec/changes/archive/                    # журнал изменений: как спека менялась
```

## Запуск загрузки (Greenplum psql)

```bash
psql -v report_dt=2026-09-23 -f sql/d_agr_cred_load.sql
```

`report_dt` — обязательный параметр запуска; запуск без него — ошибка (подстановка текущей даты запрещена).

## Продолжить работу

```bash
openspec status --change <change-name>
openspec validate --all --strict
# в Cursor: /opsx-propose → /opsx-apply → /opsx-archive
```

*Источник SQL: giga4sql/data/SQL.txt. Собрано 24.09.2026.*
