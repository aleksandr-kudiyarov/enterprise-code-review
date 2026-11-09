# Enterprise AI Code Review for OpenCode v1.17.7

Стартовый набор конфигурации (НЕ готовый подключённый корпоративный сервис).
Проверен JSON и YAML/frontmatter; end-to-end соединения с GitLab/Confluence
не проверялись, OpenCode CLI в среде подготовки набора не установлен.

## 1. Запуск

1. Установите и проверьте `opencode --version` (ожидается `1.17.7`).
2. Используйте эту папку как отдельный проект: `cd enterprise-code-review && opencode`.
3. Настройте утверждённую корпоративную модель и провайдера через OpenCode.
   Важно: не отправляйте конфиденциальный код во внешний неразрешённый API.
4. Настройте доступ к GitLab MCP. Его официальный адрес для Self-Managed:
   `https://gitlab.sovcombank.group/api/v4/mcp` (при наличии GitLab 18.6+ и
   включённого MCP). После проверки установите `mcp.gitlab.enabled=true`.
   Если поддерживается OAuth: `opencode mcp auth gitlab`; затем `opencode mcp list`.
5. Настройте внутренний Confluence MCP в `opencode.json` (пример URL —
   плейсхолдер; не является работающим сервисом). После проверки включите его.
   Confluence Data Center потребует совместимый MCP-адаптер к REST API.
6. Установите одобренные внешние skills с фиксированными commit SHA (см. ниже).
7. В TUI вызовите `/code-review <Confluence-URL> <MR-URL-1> <MR-URL-2>`;
   можно опустить Confluence URL.

## 2. Установка внешних skills

Скачайте **полные директории skills вместе с references/** из проверенных
коммитов официальных репозиториев, разместите в `.opencode/skills/`:

- `dotnet/skills`: `plugins/dotnet/skills/csharp-expert/`
- `dotnet/skills`: `plugins/dotnet-aspnetcore/skills/dotnet-webapi/`
- `dotnet/skills`: `plugins/dotnet-diag/skills/analyzing-dotnet-performance/`
- `microsoft/postgres-skills`: `plugin/skills/postgresql-best-practices/`

`postgresql-best-practices` и `csharp-expert` — routing skills. Нужные дочерние
эксперты и references должны быть установлены, если ими пользуется анализ.
Не считать перечисленные внешние skills установленными, пока не скопированы
и не обнаружены OpenCode.

## 3. Security

- MCP-серверы отключены до настройки. Включайте только после согласования.
- Оптимально: read-only account + контролируемый MCP proxy, фильтрующий
  опасные действия на уровне инструмента. Разрешения OpenCode недостаточны
  для надёжной изоляции от всех side effects при использовании MCP.
- `edit=deny`, `bash=ask`: агент не может менять файлы штатным edit, но shell
  при разрешении способен менять файлы. Не разрешайте команды из недоверенного
  репозитория без sandbox; для максимальной безопасности установите `bash=deny`.
- Не используйте `--auto` и не публикуйте findings автоматически.
- Если нужен анализ локального source code, подготавливайте READ-ONLY
  Git worktrees и запускайте тесты в одноразовом контейнере с ограниченной сетью.
- Для cross-repo сценариев проверяйте совместимый набор commit SHA.

## 4. Ограничения стартового комплекта

- GitLab и Confluence MCP не проверены на корпоративных доменах.
- Форматы возвращаемых инструментами данных и точный состав toolset
  зависят от версии корпоративного GitLab и Confluence MCP adapter.
- Сборка/тестирование не автоматизированы: только после согласования sandbox.
- Скрипты копирования внешних skills и репозиториев намеренно не включены,
  чтобы не выполнять недоверенный код автоматически.

Документация: https://docs.opencode.ai/docs/agents/,
https://docs.opencode.ai/docs/skills/,
https://docs.opencode.ai/docs/mcp-servers/,
https://docs.gitlab.com/user/model_context_protocol/mcp_server/
