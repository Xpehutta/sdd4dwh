# Tasks

- [x] Прочитать требование «Цессия» как дерево решений и разметить покрытие сценариями
- [x] Выбрать непокрытую ветвь: фильтр на счёт покупки (опаснее, чем `max()` на уровне 3)
- [x] Записать «почему это опасно» до правки файлов
- [x] `openspec new change cession-edge-purchase-filter`
- [x] Delta: `## MODIFIED Requirements` с полным текстом требования и обоими существующими сценариями дословно
- [x] Два новых сценария: `meas_rub = 0` и `meas_cd <> 'AGRA_L'`
- [ ] `openspec validate cession-edge-purchase-filter --strict`
- [ ] Archive
- [ ] PR, CI green, merge
