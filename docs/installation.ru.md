[Установка](installation.ru.md) | [English](installation.md)

# Установка

nachisto — скилл в формате каталога. Устанавливайте весь каталог, а не только `SKILL.md`: workflow правки читает `references/patterns.md`.

Для runtime скилла зависимости не нужны. Git нужен для clone и archive-маршрутов, Python 3.9 или новее — для тестового harness, Node.js 22.20.0 или новее — для закреплённого Skills CLI.

## Выберите маршрут

| Агент или поверхность | Рекомендуемый маршрут | Доказательство |
| --- | --- | --- |
| Любой хост Agent Skills | Клон в каталог пользовательских скиллов | Переносимая структура; поведение хоста зависит от реализации |
| Claude Code | Плагин из marketplace | Официальный маршрут; live-поведение 2.5.1 не проверено |
| Qoder CLI или IDE | Клон в `~/.qoder/skills/nachisto` | Официальный путь; live-поведение 2.5.1 не проверено |
| Приложение Qoder | Загрузить ZIP из шести runtime-файлов | Официальный маршрут; live-поведение 2.5.1 не проверено |
| Qoder Cloud Agents | Загрузить ZIP из шести runtime-файлов через Skills API | Официальный API-маршрут; live-поведение 2.5.1 не проверено |
| Cursor | Клон в `~/.cursor/skills/nachisto` | Официальный путь; live-поведение 2.5.1 не проверено |
| claude.ai | Загрузить ZIP из шести runtime-файлов | Официальный маршрут custom skill; live-поведение 2.5.1 не проверено |
| MiniMax, Codex, pi и другие CLI | Закреплённый `skills` CLI | Цель документирована `skills@1.9.0`; поведение хоста не проверено |
| ZCode | Клон в `~/.zcode/skills/nachisto` и Refresh | Официальный путь; live-поведение 2.5.1 не проверено |

Значение уровней доказательств описано в [матрице совместимости](compatibility.ru.md).

## Универсальный клон

Для агентов, которые ищут пользовательские скиллы в `~/.agents/skills/`:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/S1lverElixir/nachisto ~/.agents/skills/nachisto
```

В результате должна получиться структура:

```text
~/.agents/skills/nachisto/
├── SKILL.md
├── references/
│   └── patterns.md
├── LICENSE
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
└── .cursor-plugin/
    └── plugin.json
```

Не устанавливайте скилл поверх каталога с посторонними файлами. Для управляемого развёртывания закрепите проверенный релиз или commit, а не движущуюся ветку.

## Claude Code

Добавьте репозиторий как marketplace и установите плагин:

```text
/plugin marketplace add S1lverElixir/nachisto
/plugin install nachisto@nachisto
```

Marketplace описан в `.claude-plugin/marketplace.json`. Манифест плагина указывает на корень репозитория через `skills: ["./"]`.

Если `/plugin` недоступна, обновите Claude Code и сверьте требования с текущей [документацией плагинов](https://code.claude.com/docs/en/plugins).

## Qoder

### CLI и IDE

Склонируйте репозиторий в каталог пользовательских скиллов Qoder:

```bash
mkdir -p ~/.qoder/skills
git clone https://github.com/S1lverElixir/nachisto ~/.qoder/skills/nachisto
```

Qoder CLI загружает новые скиллы при старте. В текущей сессии выполните:

```text
/skills reload
/skills
```

Qoder IDE требует перезапуска. Введите `/` в чате и найдите `nachisto`.

Источники: [Qoder CLI Skills](https://docs.qoder.com/cli/Skills), [Qoder IDE Skills](https://docs.qoder.com/extensions/skills).

### Приложение Qoder

Соберите ZIP по инструкции ниже и откройте **Extensions → Skills → Add Skills → Upload Skill**. Можно загрузить один ZIP, `.skill` или `SKILL.md`; папку загрузить напрямую нельзя. Используйте ZIP, чтобы включить `references/patterns.md`.

Qoder также обнаруживает скиллы в `~/.agents/skills`. После установки убедитесь, что `nachisto` появился в **Installed**, вызовите его через `/` и запустите небольшой тест.

Источник: [Qoder Skills](https://docs.qoder.com/qoder/skills).

### Qoder Cloud Agents

Cloud Agents принимает ZIP или multipart file tree с одной верхнеуровневой папкой `nachisto/`. Соберите архив из шести runtime-файлов по инструкции ниже и загрузите его через Skills API:

```bash
curl -X POST https://api.qoder.com/api/v1/cloud/skills \
  -H "Authorization: Bearer $QODER_ACCESS_TOKEN" \
  -F "files=@nachisto.zip"
```

Привяжите полученный skill ID к агенту через Cloud Agents API. Доступ может требовать early access; live-поведение 2.5.1 не проверено. Архив и распакованное содержимое должны быть не больше 50 MB каждое.

Источник: [Qoder Cloud Agents Skills](https://docs.qoder.com/cloud-agents/skills).

## Cursor

Склонируйте репозиторий в каталог пользовательских скиллов Cursor:

```bash
mkdir -p ~/.cursor/skills
git clone https://github.com/S1lverElixir/nachisto ~/.cursor/skills/nachisto
```

Cursor обнаруживает скиллы при старте. Откройте **Customize → Skills** или введите `/nachisto` в Agent chat. Для проектной установки используйте `.agents/skills/nachisto/` или `.cursor/skills/nachisto/`.

В репозитории также есть `.cursor-plugin/plugin.json`. Cursor считает корневой `SKILL.md` single-skill plugin, если нет каталога `skills/` и поля `skills` в манифесте. Live-поведение локального плагина 2.5.1 не проверено.

Источники: [Cursor Skills](https://cursor.com/docs/skills), [Cursor Plugins](https://cursor.com/docs/reference/plugins).

## claude.ai

Соберите ZIP так, чтобы внутри была одна верхнеуровневая папка `nachisto/`:

```bash
git clone https://github.com/S1lverElixir/nachisto nachisto
git -C nachisto archive --format=zip --prefix=nachisto/ -o ../nachisto.zip \
  HEAD SKILL.md LICENSE references/patterns.md \
  .claude-plugin/plugin.json .claude-plugin/marketplace.json .cursor-plugin/plugin.json
```

В архиве не должно быть документации, тестов, Git-метаданных или обёртки `nachisto-main/`. Имя папки должно совпадать с именем скилла.

В claude.ai загрузите ZIP через **Customize → Skills → + → Create skill → Upload a skill**. В аккаунтах с unified rollout используйте **Plugins → +**; названия элементов могут отличаться. Skills/Plugins, code execution и file creation должны быть включены в настройках аккаунта или через политику организации.

Standalone ZIP-upload в Claude Desktop не подтверждён. Для Claude Desktop и claude.ai документирован отдельный plugin-bundled маршрут; его live-поведение 2.5.1 здесь не проверено.

Источники: [Use skills in claude.ai](https://support.claude.com/en/articles/12512180-use-skills-in-claude), [Create custom skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills).

## Skills CLI

Версия установщика закреплена, чтобы команда не менялась незаметно:

```bash
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent <agent>
```

Примеры:

```bash
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent minimax-code
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent codex
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent pi
```

Список целей закреплённой версии находится в [docs/agents.md](https://unpkg.com/skills@1.9.0/docs/agents.md). Документированная цель CLI доказывает маршрут установки, но не live-совместимость nachisto с этим хостом.

По умолчанию CLI отправляет anonymous telemetry. Чтобы отказаться, задайте `DISABLE_TELEMETRY=1`. См. [Skills CLI reference](https://www.skills.sh/docs/cli).

## ZCode

Склонируйте репозиторий в каталог пользовательских скиллов ZCode:

```bash
mkdir -p ~/.zcode/skills
git clone https://github.com/S1lverElixir/nachisto ~/.zcode/skills/nachisto
```

Откройте **Settings → Skills**, нажмите **Refresh** и убедитесь, что скилл включён. ZCode документирует `~/.zcode/skills/` и проектный `.zcode/skills/`; `.agents/skills` также поддерживается, но глобальный `~/.agents/skills/` отдельно не подтверждён.

Источник: [ZCode Skills](https://zcode.z.ai/en/docs/skill).

## Ручное копирование

Скопируйте весь каталог репозитория в каталог пользовательских скиллов агента:

```bash
mkdir -p ~/.agents/skills
cp -R ./nachisto ~/.agents/skills/nachisto
```

Используйте этот маршрут, только если хост документирует такой каталог. Сохраняйте все runtime-файлы вместе и не перезаписывайте постороннее содержимое.

## Проверка установки

1. Убедитесь, что хост показывает `nachisto` и его description.
2. Используйте ручной вызов хоста: `/nachisto` для standalone skills, `/nachisto:nachisto` для плагина Claude Code или `$nachisto` для ZCode.
3. Запустите безопасную paste-правку и проверьте финальный блок.
4. Убедитесь, что просьба сгенерировать или перевести текст не запускает humanizer-workflow.
5. В файловом режиме проверьте, что агент запрашивает отдельное разрешение на запись после diff.

Успешное обнаружение скилла не доказывает качество ответов. Live-приёмка требует прогона замороженной матрицы на установленном payload.

## Обновление

Для clone-установки:

```bash
git -C ~/.agents/skills/nachisto pull --ff-only
```

Замените путь для Qoder, Cursor или ZCode. Marketplace- и CLI-установки обновляются хостом или повторной установкой. Закреплённый CLI также предоставляет `skills check` и `skills update`; перед использованием в автоматизации сверьте актуальный command reference.

После обновления всегда проверяйте версию в `SKILL.md` и plugin manifests. Автоматическое фоновое обновление не обещается.

## Управляемое развёртывание и Team/Enterprise

Это публичный репозиторий. Он не подходит для Claude Team/Enterprise **Sync from GitHub**, которому нужен private или internal repository на github.com. Автосинхронизация также требует включённых Cowork и Skills, установленного Claude GitHub App, прав repository-admin, роли Owner/Primary Owner или Enterprise custom role с правом управления библиотекой, а также одобренного разрешения **Webhooks (Read & Write)**. Если автосинхронизация включена, её запускает merge PR с version bump в default branch; ручной **Re-sync** работает отдельно.

Для управляемого распространения используйте проверенную внутреннюю копию, закреплённый commit или утверждённый организацией release archive. Не предполагайте, что каждый push обновляет все установленные агенты.

Источник: [Manage plugins for an organization](https://support.claude.com/en/articles/13837433-manage-plugins-for-your-organization).
