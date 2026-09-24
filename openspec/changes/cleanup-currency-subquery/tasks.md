# Tasks: cleanup-currency-subquery

## 1. SQL

- [x] 1.1 Заменить подзапрос `coa_crncy`: `GROUP BY (agr_cred_id, meas_cd)` + `HAVING (min = max)`; убрать `rn/mn/mx` и `ORDER BY NULL::text`
- [x] 1.2 Статическая проверка: в файле не осталось `ORDER BY NULL::text` (grep)

## 2. Проверка

- [x] 2.1 Сценарии спеки: «Неоднозначная валюта счёта» → фолбэк; «Повторяющиеся строки с одной валютой» → стабильный результат
- [x] 2.2 `openspec validate cleanup-currency-subquery --strict` — успешно
