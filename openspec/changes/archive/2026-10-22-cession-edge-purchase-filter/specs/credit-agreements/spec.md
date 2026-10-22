# Delta: credit-agreements

## MODIFIED Requirements

### Requirement: Цессия (cession_dt и флаги)

`cession_dt` SHALL определяться каскадом: (1) дата открытия счёта покупки цессии (счета `478%` или `44x11`, `meas_cd = 'AGRA_L'`, `meas_rub > 0`), если счёт есть; (2) иначе — `eks_cession_dt`; (3) иначе — из счёта проданной цессии (`4742325`): `cess_debt_coa_open_dt`, если она не раньше даты привязки счёта `coa_bind_dt`, иначе `coa_bind_dt`; (4) иначе — `cess_last_reg_dt`. `cession_buy_flag = 'Y'` SHALL быть при наличии счёта покупки, иначе `'N'`. `cess_deferred_flag = 'Y'` SHALL быть, когда есть продажа или регистрация цессии и нет покупки, иначе `'N'`.

#### Scenario: Покупка цессии

- **WHEN** по договору найден счёт покупки цессии
- **THEN** `cession_dt = cess_buy_coa_open_dt`, `cession_buy_flag = 'Y'`, `cess_deferred_flag = 'N'`

#### Scenario: Продажа без покупки

- **WHEN** счёта покупки нет, но есть счёт продажи или `cess_last_reg_dt`
- **THEN** `cession_buy_flag = 'N'`, `cess_deferred_flag = 'Y'`

#### Scenario: Счёт 478% не проходит фильтр покупки

- **WHEN** у договора есть счёт группы `478%` с `meas_cd = 'AGRA_L'` и `meas_rub = 0`, `eks_cession_dt` заполнена, счёта продажи нет и `cess_last_reg_dt` пуста
- **THEN** счёт не признаётся счётом покупки: `cession_buy_flag = 'N'`, `cess_deferred_flag = 'N'`, `cession_dt = eks_cession_dt`

#### Scenario: Счёт 44x11 с чужой метрикой

- **WHEN** у договора есть счёт `44x11` с `meas_cd <> 'AGRA_L'` и `meas_rub > 0`, `eks_cession_dt` заполнена, счёта продажи нет и `cess_last_reg_dt` пуста
- **THEN** счёт не признаётся счётом покупки: `cession_buy_flag = 'N'`, `cession_dt = eks_cession_dt`
