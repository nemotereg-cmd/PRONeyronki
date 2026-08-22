# Design skills & MCP

Дизайн-инструменты, подключённые к этому репозиторию.

## 1. UI/UX Pro Max (skills)

Источник: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
Установлено через `npx ui-ux-pro-max-cli@latest init --ai claude` (v2.15.0).

Файлы лежат в `.claude/skills/` и активируются автоматически, когда задача
касается UI/UX (страницы, компоненты, палитры, типографика, доступность, графики).

Установленные скиллы:

| Скилл | Назначение |
| --- | --- |
| `ui-ux-pro-max` | Основная база: стили, палитры, шрифтовые пары, UX-гайдлайны, чарты, 22 стека |
| `design` | Общий дизайн-воркфлоу |
| `design-system` | Генерация дизайн-системы (токены, шкалы, компоненты) |
| `brand` | Брендинг, айдентика |
| `ui-styling` | Стилизация компонентов |
| `banner-design` | Баннеры и рекламная графика |
| `slides` | Презентации |

Требуется Python 3 (только стандартная библиотека, сеть не используется).

Примеры прямого вызова поиска:

```bash
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "fintech banking" --design-system -p "MyApp"
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "glassmorphism" --domain style
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "elegant serif" --domain typography
```

Обновление: `npx ui-ux-pro-max-cli@latest update`

## 2. 21st MCP (бывший Magic MCP)

Источник: https://github.com/21st-dev/magic-mcp

Репозиторий `magic-mcp` (`@21st-dev/magic`) больше не является самостоятельным
сервером — это тонкий прокси к унифицированному **21st MCP**. Поэтому подключён
сразу целевой HTTP-сервер: см. `.mcp.json`.

```json
{
  "mcpServers": {
    "21st": {
      "type": "http",
      "url": "https://21st.dev/api/mcp",
      "headers": { "x-api-key": "${TWENTY_FIRST_API_KEY}" }
    }
  }
}
```

### Где лежит ключ

Ключ **не хранится в репозитории**. В `.mcp.json` только подстановка
`${TWENTY_FIRST_API_KEY}`, а само значение — в локальном, игнорируемом git файле
`.claude/settings.local.json`:

```json
{
  "env": {
    "TWENTY_FIRST_API_KEY": "21st_sk_..."
  }
}
```

Этот файл добавлен в `.gitignore`. Эквивалентный вариант — просто
`export TWENTY_FIRST_API_KEY="..."` в своей оболочке перед запуском Claude Code.

Новый ключ выпускается на https://21st.dev/mcp (старые ключи Magic сброшены и
не работают).

Инструменты сервера: `generate` (генерация UI-компонентов), `get_inspiration`,
`search` по каталогу компонентов/тем/шаблонов, `search_logo`. Старые имена
(`21st_magic_component_builder`, `logo_search` и т.д.) сервер тоже принимает.

Альтернатива через legacy-прокси, если нужен именно пакет из `magic-mcp`:

```json
{
  "mcpServers": {
    "magic": {
      "command": "npx",
      "args": ["-y", "@21st-dev/magic@latest"],
      "env": { "TWENTY_FIRST_API_KEY": "${TWENTY_FIRST_API_KEY}" }
    }
  }
}
```
