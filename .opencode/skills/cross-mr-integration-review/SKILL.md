---
name: cross-mr-integration-review
description: Анализ зависимостей, контрактов и интеграционной совместимости связанных GitLab Merge Requests между проектами и репозиториями.
license: MIT
---
# Cross-MR integration review

1. Зафиксировать для каждого MR project, iid, base/head SHA, target/source.
2. Построить directed graph зависимостей на основе commits, API, схем,
   событий, библиотек, feature flags и deployment constraints.
3. Определить, можно ли сформировать единый временный интеграционный checkout.
   Не смешивать разные repositories и несвязанные branches наугад.
4. Проверить API contracts, shared DTO, serialization, error codes,
   событие producer/consumer, schema + application compatibility.
5. Для миграций предусмотреть expand -> deploy -> contract сценарии.
6. Описать тестируемую матрицу версий старых/новых компонентов.
7. Если собрать совместимое состояние нельзя, оформить Coverage gap.
8. Каждая проблема должна ссылаться на конкретные MR и доказательства.
