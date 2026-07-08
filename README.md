# Fractal MCP plugin (Claude Code)

Плагин даёт агенту доступ к задачам Fractal (`tasks.bos.pro`): создание, чтение,
обновление и закрытие задач через MCP, плюс браузер-логин одним тулом. MCP-сервер
**забандлен внутрь** плагина (один файл `dist/fractal-mcp.mjs`) — ни npm, ни сборки не нужно.

## Установка (Claude Code)

```
/plugin marketplace add agiens/fractal-mcp-plugin
/plugin install fractal@fractal
```

Потом:
```
/fractal auth        # откроет браузер, залогинишься, токен сохранится сам
```
И дальше: «создай задачу X и переведи её в done» — увидишь на доске `tasks.bos.pro`.

> Плагины поддерживает только **Claude Code**. Для Cursor / Claude Desktop подключайте
> MCP-сервер напрямую (см. основной репозиторий, вариант `npx`).

## Тулы

| Tool | Что делает |
|---|---|
| `fractal_login` | Браузер-логин, сохраняет scoped-токен в `~/.fractal` |
| `fractal_list_tasks` | Загрузить поддерево задач |
| `fractal_create_task` | Создать задачу |
| `fractal_update_task` | Обновить задачу (в т.ч. `column_id: "done"`) |
| `fractal_delete_task` | Удалить задачу |

## Обновление сервера

`dist/fractal-mcp.mjs` — собранный бандл из `mcp-server/` основного репозитория:
```bash
npx esbuild mcp-server/src/index.ts --bundle --platform=node --format=esm \
  --target=node18 --outfile=dist/fractal-mcp.mjs
```
