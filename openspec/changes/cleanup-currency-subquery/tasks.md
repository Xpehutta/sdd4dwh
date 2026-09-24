# Tasks: cleanup-currency-subquery

## 1. SQL

- [ ] 1.1 Заменить подзапрос `coa_crncy`: `GROUP BY (agr_cred_id, meas_cd)` + `HAVING (min = max)`; убрать `rn/mn/mx` и `ORDER BY NULL::text`
- [ ] 1.2 Статическая проверка: в файле не осталось `ORDER BY NULL::text` (grep)

## 2. Проверка

- [ ] 2.1 Сценарии спеки: «Неоднозначная валюта счёта» → фолбэк; «Повторяющиеся строки с одной валютой» → стабильный результат
- [ ] 2.2 `openspec validate cleanup-currency-subquery --strict` — успешно
