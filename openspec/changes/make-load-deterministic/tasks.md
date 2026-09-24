# Tasks: make-load-deterministic

## 1. SQL

- [x] 1.1 Заменить оба вхождения `('now'::text)::date` на `:'report_dt'::date` в `sql/d_agr_cred_load.sql`
- [x] 1.2 Добавить комментарии о параметре (шапка файла и у изменённого предиката)

## 2. Регламент

- [x] 2.1 Зафиксировать форму запуска: `psql -v report_dt=YYYY-MM-DD` (design + README)
- [x] 2.2 Зафиксировать правило: запуск без `report_dt` — ошибка; подстановка текущей даты запрещена

## 3. Проверка

- [x] 3.1 Статическая проверка: в `sql/d_agr_cred_load.sql` не осталось `now(` и `current_date`
- [x] 3.2 `openspec validate make-load-deterministic --strict` — успешно
