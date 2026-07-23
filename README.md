# Fractal MCP (Claude Code + Claude Desktop)

**v0.2.0** (2026-07-23, собран из origin/main `bf90c6e5`) — включает стендап-фикс TPMC-10
(лимит комментариев + instruction-first header в `fractal_get_task`). plan-gate НЕ включён,
появится следующей версией.

Даёт агенту доступ к задачам Fractal (`tasks.bos.pro`): навигация по дереву, чтение,
создание/обновление, зависимости, перемещение, поиск — плюс браузер-логин одним тулом.
MCP-сервер **забандлен в один файл** — ни npm, ни сборки на стороне пользователя не нужно.

## Установка (Claude Code)

```
/plugin marketplace add k34090705-create/fractal-mcp-plugin
/plugin install fractal@fractal
```

Потом:
```
/fractal auth        # откроет браузер, залогинишься, токен сохранится сам
```
И дальше: «создай задачу X и переведи её в done» — увидишь на доске `tasks.bos.pro`.

## Установка (Claude Desktop)

Плагины-маркетплейс — это только Claude Code. Для Desktop есть готовый `.mcpb`-бандл:

1. Скачай [`desktop/fractal-mcp-0.2.0.mcpb`](desktop/fractal-mcp-0.2.0.mcpb).
2. Открой Claude Desktop → **Settings → Extensions** и перетащи туда файл (или дважды кликни по нему) → **Install**.
3. Вызови тул `fractal_login` — откроется браузер, токен сохранится в `~/.fractal`.

Node ставить не нужно — рантайм внутри Desktop. Токен общий с Claude Code (`~/.fractal/config.json`),
так что если уже логинился в Code — Desktop подхватит его сам.

## Тулы

| Tool | Что делает |
|---|---|
| `fractal_login` | Браузер-логин, сохраняет scoped-токен в `~/.fractal` |
| `fractal_get_subtree` | Компактный digest поддерева — рекомендуемая навигация (дёшево по контексту) |
| `fractal_get_task` | Одна задача целиком (content) + комментарии |
| `fractal_search` | Поиск задач по заголовку внутри scope токена |
| `fractal_list_tasks` | Полный дамп поддерева (тяжёлый — крайняя мера) |
| `fractal_create_task` | Создать задачу |
| `fractal_update_task` | Обновить задачу (в т.ч. `column_id: "done"`) |
| `fractal_add_comment` | Добавить комментарий к задаче |
| `fractal_add_dependency` | Зависимость blocker → blocked (или снять её) |
| `fractal_move_task` | Переместить задачу (родитель / lane) |
| `fractal_delete_task` | Удалить задачу |

## Обновление сервера

Оба бандла (`dist/fractal-mcp.mjs` для Code и `desktop/server/index.mjs` для Desktop) —
собранный esbuild-бандл из `mcp-server/` основного репозитория. Пересборка:

```bash
# из origin/main основного репо (локальный main бывает отстаёт)
npx esbuild mcp-server/src/index.ts --bundle --platform=node --format=esm \
  --target=node18 --outfile=dist/fractal-mcp.mjs
cp dist/fractal-mcp.mjs desktop/server/index.mjs

# пересобрать .mcpb для Desktop
cd desktop && npx @anthropic-ai/mcpb pack . fractal-mcp-0.2.0.mcpb
```
