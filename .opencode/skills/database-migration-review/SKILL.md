---
name: database-migration-review
description: Проверка PostgreSQL SQL-миграций на блокировки, длительность, транзакционность, безопасность данных и совместимость со старыми и новыми версиями приложений.
license: MIT
---
# Database migration safety

- Проверить версию PostgreSQL, framework миграций, idempotency assumptions,
  транзакционный режим и зависимость от соседних MR.
- Оценить lock levels, большие таблицы, долгий backfill, batched changes,
  concurrent index, constraints, NOT NULL и default values.
- Помнить: CREATE INDEX CONCURRENTLY не допускается внутри транзакционного блока;
  проверять поддержку framework и фактическую транзакционность.
- Для DROP/RENAME/TYPE изменений проверить staged expand/contract и клиентские версии.
- Проверить не только rollback, но и forward recovery при частичном применении.
- Не выполнять DDL/DML на production. EXPLAIN ANALYZE запускает оператор:
  для потенциально изменяющего данные SQL — только безопасная тестовая среда.
- Для каждого риска указать фактическое место SQL, масштаб, сценарий и фикс.
