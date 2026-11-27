[Совместимость](compatibility.ru.md) | [English](compatibility.md)

# Совместимость и доказательства

Документированный маршрут установки не равен проверенному поведению runtime. Эта матрица показывает, что фактически проверено для nachisto 2.5.1.

## Уровни доказательств

| Уровень | Значение |
| --- | --- |
| A | Live-установка и поведение проверены в названной версии и среде. |
| B | Offline package smoke пройден: структура, состав и проверки после распаковки. |
| C | Официальная документация и статические проверки маршрута или структуры; live-поведение не проверено. |
| D | Не проверено. |

Исторический A для 2.4.x не означает A для 2.5.x. Результат относится к точному payload hash и среде, записанным в журнале проверок.

## Текущая матрица

| Маршрут или платформа | Установка/пакет | Live-поведение 2.5.1 |
| --- | --- | --- |
| Переносимый пакет | B | D — package smoke не оценивает ответ модели |
| Клон в `~/.agents/skills/` | B — структура пакета | D |
| Claude Code marketplace | C — официальные доки и статические манифесты | D |
| claude.ai custom skill ZIP | C — официальные доки, ZIP layout, лимит description | D — исключён из этого live-цикла решением пользователя |
| Claude Desktop custom skill upload | D — standalone upload не подтверждён | D |
| Claude Desktop plugin-bundled skill | C — официальные доки плагинов | D |
| Cursor standalone skill | C — официальные доки и пользовательский путь | D |
| Cursor local plugin | C — корневой single-skill plugin | D |
| Qoder CLI и IDE | C — официальные пути и reload | D |
| Приложение Qoder, upload | C — официальный ZIP/`.skill`/`SKILL.md` маршрут | D |
| Qoder Cloud Agents package | C — официальный package-маршрут | D |
| MiniMax Code через Skills CLI | C — цель указана в `skills@1.9.0` | D |
| ZCode | C — официальный путь и Refresh | D |
| Codex и pi через Skills CLI | C — цели указаны в `skills@1.9.0` | D |
| Mistral web skill editor и Vibe | D / NOT RUN для текущего payload | D / NOT RUN |
| Kimi web | D / NOT RUN — аутентификация не подтверждена | D / NOT RUN |
| Другие агенты | D — зависит от цели и хоста | D |
| Claude Team/Enterprise GitHub sync | D — публичный репозиторий не подходит | D |

## Текущие блокеры

### Mistral

Текущий skill hash: `8630ce583d0c2063e117407a9fbf067e6877d942a68e441991f27ffa5a040eda`. Payload regenerated; буквальное извлечение install-маркеров воспроизводит этот hash.

Текущий payload 2.5.1 не отправлялся для install/save/reload или fixture-матрицы. Историческая попытка 2.5.0 была заблокирована ошибками браузерного транспорта, включая `COMMAND_DRAINING`; замена или сохранение не подтверждены. Исторические доказательства установки не оценивают текущий payload.

Историческое evidence: [browser transport status](../tests/runs/mistral-2026-11-26-r3/browser-transport-status.md).

### Kimi

Аутентификация и capacity для текущего payload не подтверждены. Текущий payload не отправлялся, поэтому текущего install или behavioral verdict нет.

Историческое наблюдение: [Kimi status](../tests/runs/kimi-2026-11-26-r1/current-status.md) относится к payload `fae10717…`, а не к `8630ce…`. Оно не проверяет текущий hash.

### Динамический файловый режим

F01/F02 требуют OS-enforced sandbox без сети, где запись разрешена только в scratch. Такая среда недоступна, поэтому тесты остаются **BLOCKED**. Рабочий репозиторий и пользовательские файлы не используются как полигон.

### Внешние сканы и каталоги

Публичная страница Snyk по-прежнему показывает аудит от 23.11.2026 и старый snapshot скилла. Текущий SHA не просканирован. Во время аудита индекс skills.sh был stale; публикация release candidate сама по себе не доказывает обновление индекса или скана.

## Что эта матрица не утверждает

- Она не утверждает, что каждая перечисленная платформа проверена live.
- Она не превращает документированную цель CLI в совместимость с хостом.
- Она не считает исторические payload доказательством для текущего hash.
- Она не считает offline evidence integrity семантическим review.
- Она не разрешает стабильный релиз, пока live-приёмка не завершена.

Release acceptance gate остаётся **CLOSED**. См. [tests/RESULTS.md](../tests/RESULTS.md) для журнала прогонов и [tests/README.ru.md](../tests/README.ru.md) для критериев приёмки.
