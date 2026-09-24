# Tasks: add-d-agr-cred-load

## 1. Baseline

- [x] 1.1 Зафиксировать SQL-загрузку в репозитории (`sql/d_agr_cred_load.sql`, как есть)
- [x] 1.2 Зафиксировать регламент полной перезагрузки и перечень DQ-проверок (см. design)

## 2. Спека

- [x] 2.1 Delta-спека: потоки, ключ, атрибуты (`issue_dt` / `crncy_id` / цессия / ВДО), регламент
- [x] 2.2 `openspec validate add-d-agr-cred-load --strict` — проверка проходит
- [x] 2.3 Ревью-гейты: proposal → specs → приёмка (в симуляции — ревью автора)
