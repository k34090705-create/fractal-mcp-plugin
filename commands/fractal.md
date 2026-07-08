Работа с задачами Fractal (tasks.bos.pro) через MCP-сервер `fractal` (тулы `mcp__fractal__*`).

Формы:

- `/fractal auth` — войти: вызови тул `mcp__fractal__fractal_login`. Он откроет браузер на
  tasks.bos.pro, возьмёт твою сессию, выпустит scoped-токен и сохранит его в `~/.fractal`.
  Ничего копировать не нужно. После — подтверди доступ вызовом `fractal_list_tasks`.
- `/fractal status` — вызови `fractal_list_tasks`; если вернулось поддерево — залогинен, тулы работают.
- `/fractal <что сделать с задачами>` — выполни просьбу тулами:
  - создать → `fractal_create_task` (`{title, content?, parentId?}`)
  - изменить → `fractal_update_task` (`{taskId, updates}`)
  - закрыть → `fractal_update_task` с `updates: {column_id: "done"}`
  - удалить → `fractal_delete_task`
  - прочитать → `fractal_list_tasks`

`$ARGUMENTS` — первое слово подкоманда (`auth`/`status`) либо сразу текст задачи.

Если любой дата-тул вернул «Не залогинен» — сначала вызови `fractal_login` (или подскажи `/fractal auth`).
Никогда не печатай widget-токен или содержимое `~/.fractal/config.json` в чат.
