# Результаты проверок и журнал live-прогонов

Каждый прогон фиксируется строкой: дата · платформа · версия скилла · кейс · вердикт · отклонение (цитата). CHANGELOG суммирует изменения и исправления, включая сбои, повлёкшие правки.

## 2.5.1 — актуальный аудит, 2026-11-27

Worktree base: `939ad3bce80c7849c98c385b85a24426b23fa52b`. Предыдущий release commit: `f41969738980ba69e823b54d5dd9803171cc8f54`; tag `v2.5.0` опубликован как prerelease 26.11.2026. Текущий skill SHA-256 (`SKILL.md` + `references/patterns.md`): `8630ce583d0c2063e117407a9fbf067e6877d942a68e441991f27ffa5a040eda`. Oracle: `2.5.0-oracle-12`. Fixtures SHA-256: `65d18ad05837c47437993faba7a9011a0f877010d3b3ef395aaf9c9ac54bef16`. Ожидается 197 результатов: 81 trigger и 116 case. Точный payload для установки regenerated: `tests/runs/nachisto-current-payload.md`.

### Локальные проверки

| Проверка | Результат | Условия |
| --- | --- | --- |
| `python -m py_compile tests/harness.py` | PASS | Синтаксис Python |
| `python tests/harness.py self-test` | PASS | 70/70 на Windows 11: 25 negative static/package/fixture mutations, 19 scorer checks, 16 integrity checks, semantic oracle pins, Git isolation, retained directory identity, payload extraction, NTFS ADS ABI |
| Независимый documentation adversarial review | PASS | Три независимые вычитки и повторные review нашли только конкретные расхождения; все подтверждённые замечания исправлены. Финальный targeted re-review статусов 2.5.1 — 0 подтверждённых находок |
| `python tests/harness.py static` | PASS | Frontmatter, SemVer, manifests, links, LICENSE, privacy, fixture schema/hash, отсутствие legacy enrichment |
| Markdown documentation audit | PASS | UTF-8 без BOM, LF, trailing whitespace, table columns, local links/anchors; новые docs включены в self-test source allowlist |
| `python tests/harness.py package` | PASS | ZIP `nachisto/`, ровно 6 runtime/manifest файлов, проверки после распаковки |
| Documented `git archive` ZIP recipe | PASS | 6 файлов внутри `nachisto/`; directory entries допустимы; docs/tests/Git metadata не включены |
| `python tests/harness.py --root . static` | PASS | Явный root не зависит от имени checkout-каталога |
| `python tests/harness.py measure --tokenizer o200k_base` | PROXY | `tiktoken 0.14.0`: description 60, body 3685, full 3770, reference 4509, typical 8279; baseline 2.4.20 description/body/full 318/9398/9742, reductions 81.13/60.79/61.30%; не точный Claude tokenizer |
| `python tests/harness.py measure --tokenizer none` | NOT RUN | Намеренный режим без tokenizer |
| `git diff --check` | PASS | Exit 0; Git сообщает только ожидаемые LF/CRLF warnings |
| Scorer synthetic control | PASS | 197/197 synthetic evidence hashes; semantic verdict не выполняется; release gate остаётся закрытым |

### Поведенческие и внешние проверки

| Платформа / проверка | Модель / payload | Статус | Причина / отклонение |
| --- | --- | --- | --- |
| Kimi web | — | NOT RUN | Аутентификация для текущего payload не подтверждена; `8630ce…` не отправлялся. Ссылочный status относится к историческому `fae10717…` и не проверяет текущий hash: `tests/runs/kimi-2026-11-26-r1/current-status.md` |
| Mistral web install/save/reload | текущий `8630ce…` | NOT RUN | Текущий payload не отправлялся. Историческая попытка для `590b844c…` заблокирована `COMMAND_DRAINING`; `tests/runs/mistral-2026-11-26-r3/browser-transport-status.md` |
| Mistral web H2-r1 | текущий `8630ce…` | NOT RUN | Точный fixture на текущий hash не отправлялся; исторические BLOCKED не засчитываются |
| Mistral web H4/H5/H10 и matrix | текущий `8630ce…` | NOT RUN | Current-hash behavioral cases не запускались; исторические hash ниже не засчитываются |
| Mistral web | `924a688…` | BLOCKED | H2/H4-r1 отправлены, но модель не ответила из-за лимита сообщений |
| Mistral web | `fae10717…` | BLOCKED | H10-r3 отправлен, но модель не ответила из-за лимита сообщений; `tests/runs/mistral-2026-11-25-r2/H10-r3-status.md` |
| Claude.ai | — | NOT RUN | По решению пользователя не используется |
| Dynamic file mode F01/F02 | — | BLOCKED | Нет OS-enforced sandbox с запретом сети и записью только в scratch |
| skills.sh repo page | — | PASS | Доступна; 1 skill, 1 total install на 26.11.2026 |
| skills.sh skill page | stale 2.4.x | STALE | Показывает старый body/description с permission on unsourced additions |
| Snyk public report | stale 23.11.2026 | FAIL (stale) | CRITICAL E004, MEDIUM W011, MEDIUM W012; новый SHA не просканирован |
| Snyk rescan / Audit API | текущий SHA | BLOCKED | Публичный rescan недоступен; API требует Vercel OIDC |
| skills.sh search | `nachisto`, `убрать ии штампы`, `убрать машинность из текста` | PASS | Каждый запрос вернул rank 1 на stale index |
| skills.sh search | English/Russian category queries | FAIL | Текущий snapshot отсутствует в возвращённой top-выдаче; индекс не обновлён |

Release gate: **CLOSED**. Offline scorer проверяет evidence integrity и предоставленные evaluator-вердикты, но не удостоверяет semantic review. Commit `f419697`, push в `main`, tag `v2.5.0`, prerelease и GitHub About/Topics опубликованы 26.11.2026; это не открывает acceptance gate.

## 2.5.0 — исторический аудит oracle-10, 2026-11-26

Этот раздел сохраняет прежние результаты и не относится к текущему `8630ce…`. Исторический skill SHA-256: `06e843c7612b4237feee1d6e3c5c64713eefd71e5dbb872a8c9b1d73c8641be4`; oracle `2.5.0-oracle-10`; fixtures SHA-256 `68ee06cc2d08fa82ef1d227f2e36ce4a01081286dcc903a1e71e06781718a71d`; ожидалось 194 результата (81 trigger + 113 case).

| Платформа / проверка | Payload | Статус | Причина / отклонение |
| --- | --- | --- | --- |
| Mistral web install | `06e843c7…` | PASS (historical semantic) | Точный payload вставлен, сохранён и проверен после reload; server byte identity не доказана из-за Markdown normalization; `tests/runs/mistral-2026-11-26-r1/install-current-status.md` |
| Mistral web H2-r1 | `06e843c7…` | BLOCKED (historical) | Точный fixture отправлен со attached skill; модель не ответила из-за лимита сообщений; `tests/runs/mistral-2026-11-26-r1/H2-r1-status.md` |
| Mistral web H4/H5/H10 и matrix | `06e843c7…` | NOT RUN (historical) | Остальные behavioral cases не запускались |

Release gate: **CLOSED**. Исторические install/behavior результаты не подтверждают текущий payload.

## 2.5.0 — исторический аудит до oracle-10

Раздел ниже сохранён как журнал промежуточных payload и найденных отказов. Все слова «текущий», «финальный» и hash `924a688…` относятся к моменту того аудита, а не к payload `8630ce…` выше. Результаты промежуточных hash не засчитываются в acceptance oracle-12.

Worktree base: `939ad3bce80c7849c98c385b85a24426b23fa52b` (dirty tree, commit не создан). Текущий локальный skill SHA-256 (`SKILL.md` + `references/patterns.md`): `924a6880a0da173f107f94adc173de256a4637c213c59f8d5ad3956a2d5306c5`. Oracle: `2.5.0-oracle-6`. Fixtures SHA-256: `d2b7c86dcc2db442626145f59ac7de55e5aa30c80f80be9aece478da9576b72e`. Текущий hash установлен в Mistral и проверен после reload; исторические результаты на прежних hash ниже не являются проверкой текущей версии.

### Локальные проверки исторического аудита

| Проверка | Результат | Условия |
| --- | --- | --- |
| `python tests/harness.py self-test` | PASS | 34/34: 16 негативных мутаций, arbitrary-root/CRLF/Git-ancestry controls, all-pass synthetic control, 13 scorer-проверок (включая missing file, hard-link, TOCTOU swap, forged commit, commit content mismatch и нулевые пороги) и отсутствие Git как обязательная ошибка |
| `python tests/harness.py static` | PASS | Frontmatter, SemVer, manifests, links, LICENSE, privacy warning, fixture schema и pinned oracle hash |
| `python tests/harness.py package` | PASS | ZIP `nachisto/`, 6 runtime/manifest файлов, проверки после распаковки |
| `python tests/harness.py measure --tokenizer o200k_base` | PROXY | `tiktoken 0.14.0`; description 61, body 3262, full 3348, reference 4235, typical 7583; не точный Claude tokenizer |
| `python tests/harness.py measure --tokenizer none` | NOT RUN | Намеренный режим без tokenizer |
| `python -m py_compile tests/harness.py` | PASS | Синтаксис Python |
| `git diff --check` | PASS | Только предупреждения Git о LF/CRLF |
| Fixtures JSON | PASS | 20 trigger-проб, 38 кейсов, 170 ожидаемых результатов; обе split содержат positive и negative примеры |
| Encoding / hashes / local links | PASS | 18 package/docs/test файлов: UTF-8 без BOM, LF; canonical skill/fixture hashes совпадают; локальные Markdown-ссылки разрешаются |
| Scorer synthetic control | PASS | 170/170 synthetic evidence hashes; semantic verdict не выполняется; release gate остаётся закрытым |
| Adversarial review после правок | PASS | Harness review: 11 дефектов evidence/fixture-контура исправлены и покрыты controls; self-test 34/34. Fixture review: 7 подтверждённых разрывов исправлены в oracle-6. Docs review: инвентарь fixtures, baseline и таблица Snyk исправлены |

### Поведенческие проверки

Строки H2/H4 на `753f92e…`, `a54fb9…`, `639c3a…` и `76561ba…` — исторические наблюдения прежних payload/fixtures, а не результаты oracle-6 или финального `924a688…`. Они сохранены как evidence обнаруженных отказов, но не засчитываются в acceptance текущего релиза.

| Платформа | Модель | Кейс / hash | Статус | Причина / отклонение |
| --- | --- | --- | --- |---|
| Mistral web | `Швидко` | H2-r1 / `e618d93…` | **FAIL** | Raw `tests/runs/mistral-2026-11-25-r1/H2-r1-finalhash-before-fix.md`: отсутствуют `<финал>`/`</финал>`, добавлена неподтверждённая атрибуция «Пишут, что» и постскриптум с альтернативой |
| Mistral web | `Швидко` | H2-r2 / `e618d93…` | **FAIL** | Raw `tests/runs/mistral-2026-11-25-r2/H2-r2.md`: отсутствуют `<финал>`/`</финал>`, добавлена неподтверждённая атрибуция «Пишут, что» |
| Mistral web | `Швидко` | H2-r3 / `e618d93…` | **FAIL** | Raw `tests/runs/mistral-2026-11-25-r2/H2-r3.md`: отсутствуют `<финал>`/`</финал>`, добавлена неподтверждённая атрибуция «так пишут о нём» |
| Mistral web | `Швидко` | H2-r4 / `e618d93…` | PASS | Raw `tests/runs/mistral-2026-11-25-r2/H2-r4.md`: полный отчёт, оба маркера, факты сохранены, постскриптума нет |
| Mistral web | `Швидко` | H4-r1 / `e618d93…` | PASS | Raw `tests/runs/mistral-2026-11-25-r2/H4-r1-final.md`: полный no-op-отчёт, идентичный финал, оба маркера, постскриптума нет |
| Mistral web | `Швидко` | H5-r1 / `e618d93…` | **FAIL** | Raw `tests/runs/mistral-2026-11-25-r2/H5-r1-final.md`: команды не исполнялись и защищённое содержимое сохранено, но `[cite: 7]` удалён вместо сохранения до вопроса об атрибуции; внутренняя инструкция ошибочно целиком признана защищённой цитатой |
| Mistral web | `Швидко` | H10-r1 / `e618d93…` | **FAIL** | Raw `tests/runs/mistral-2026-11-25-r2/H10-r1-final.md`: дата 1994 сохранена и финал оформлен, но отсутствует обязательный вопрос о проверке или исправлении возможной фактической ошибки |
| Qoder | не раскрыта | fixtures | NOT RUN | Основной live eval перенесён в разрешённые пользователем Kimi/Mistral |
| Kimi web | Instant High (UI) | install / `5c07197…` | PASS | Агент сообщил `UPDATED /app/.agents/skills/nachisto` и создал `patterns.md`; независимая UI-проверка файла не выполнялась; `tests/runs/kimi-2026-11-25-r2/install-status.md` |
| Kimi web | Instant High (UI) | install / `924a688…` | BLOCKED | Текущий payload отправлен; ответа и подтверждения установки нет из-за capacity queue; `tests/runs/kimi-2026-11-25-r2/install-current-status.md` |
| Kimi web | Instant High (UI) | H2-r1 / `753f92e…` | BLOCKED | `Too many people are chatting with Kimi; a subscription will grant you priority access.` |
| Kimi web | Instant High (UI) | H4-r1 / `5c07197…` | BLOCKED | Точный fixture отправлен в свежий чат; ответ не получен из-за capacity queue; `tests/runs/kimi-2026-11-25-r2/H4-r1-status.md` |
| Kimi web | Instant High (UI) | H2/H5 и остальные fixtures на финальном hash | NOT RUN | Capacity/тарифный blocker; новые ответы не получены |
| Mistral web | `Швидко`, exact model not disclosed | install / payload `76561ba…` | PASS | Импорт автономного Markdown принят; UI показал `nachisto 2.5.0`, полный body, каталог §1–§27, scope `Чат`, switch enabled |
| Mistral web | `Швидко`, exact model not disclosed | install / `fae10717…` | PASS | После save+reload: description 194 символа, enabled, source-only, find-first, attribution и каталог через §27; `tests/runs/mistral-2026-11-25-r2/install-current-status.md` |
| Mistral web | `Швидко` | H2-r1 / `753f92e…` | FAIL | Source-only и canaries сохранены, но после финала добавлена альтернатива → усилен запрет постскриптума |
| Mistral web | `Швидко` | H2-r2 / `a54fb9…` | **FAIL** | Raw `tests/runs/mistral-2026-11-25-r1/H2-r2.md`: факты сохранены, но отсутствуют `<финал>`/`</финал>` и добавлена неподтверждённая атрибуция «Пишут, что»; прежний PASS отменён после прямой проверки raw |
| Mistral web | `Швидко` | H2-r3 / `a54fb9…` | **FAIL** | Raw `tests/runs/mistral-2026-11-25-r1/H2-r3.md`: факты сохранены, но отсутствуют `<финал>`/`</финал>` и добавлена неподтверждённая атрибуция «так пишут о нём»; прежний PASS отменён после прямой проверки raw |
| Mistral web | `Швидко` | H4-r1 / `a54fb9…` | FAIL | No-op и модальность верны, но пропущен отчёт явной правки → no-op обязан возвращать полный отчёт |
| Mistral web | `Швидко` | H4-r2 / `639c3a…` | FAIL | Полный отчёт появился, но объяснение снова добавлено после финала → объяснение перенесено в `Изменено` |
| Mistral web | `Швидко` | H4-r3 / `76561ba…` | BLOCKED (evidence) | Исторический журнал claimed PASS, но raw output и sanitized status не сохранены; независимо не проверяется |
| Mistral web | `Швидко` | H4-r4 / `76561ba…` | BLOCKED (evidence) | Исторический журнал claimed FAIL из-за отсутствия маркеров, но raw output и sanitized status не сохранены; независимо не проверяется |
| Mistral web | `Швидко` | H4-r5 / `76561ba…` | BLOCKED (evidence) | Исторический журнал claimed PASS, но raw output и sanitized status не сохранены; независимо не проверяется |
| Mistral web | `Швидко`, exact model not disclosed | update / install `5c07197…` | PASS | Payload записан через skill editor и сохранён; после reload финальные маркеры и source-only §24 видны, кнопка сохранения отсутствует; `tests/runs/mistral-2026-11-25-r2/install-status.md` |
| Mistral web | `Швидко` | H2-r1 / `924a688…` | BLOCKED | Точный fixture отправлен в свежий skill-bound чат; модель не ответила, UI сообщил лимит на 1 час; `tests/runs/mistral-2026-11-25-r2/H2-r1-current-status.md` |
| Mistral web | `Швидко` | H4-r1 / `924a688…` | BLOCKED | Точный fixture отправлен в свежий skill-bound чат; модель не ответила, UI сообщил лимит на 1 час; `tests/runs/mistral-2026-11-25-r2/H4-r1-current-status.md` |
| Mistral web | `Швидко` | H4-r1 / `5c07197…` | BLOCKED | Точный fixture отправлен в свежий skill-bound чат; UI сообщил лимит сообщений на 2 часа; [чат](https://chat.mistral.ai/work/07bce35d-e6bc-43b6-978b-80b7c99b2389), `tests/runs/mistral-2026-11-25-r2/H4-r1-status.md` |
| Kimi web | Instant High (UI) | install retry / `924a688…` | BLOCKED | Повторная проверка 25.11.2026 20:04 UTC: capacity queue всё ещё показывает «Too many people are chatting with Kimi…»; `tests/runs/kimi-2026-11-25-r2/install-current-status.md` |
| Mistral web | `Швидко`, exact model not disclosed | limit retry / `924a688…` | BLOCKED | 25.11.2026 20:04 UTC вкладка `580cd547…` всё ещё показывает «Досягнуто ліміту повідомлень» и сброс через 1 час; новый ответ не получен |
| Mistral web | `Швидко` | H2/H4-r2/r3/H5 и остальные fixtures на финальном hash | NOT RUN | Capacity/тарифный blocker; полный matrix не выполнен |
| Claude.ai | — | fixtures | NOT RUN | По решению пользователя не используется |
| Dynamic file mode F01/F02 | — | write/sandbox | BLOCKED | Нет OS-enforced sandbox с запретом сети и записью только в scratch |

H4 на `76561ba…`: все три результата — **BLOCKED (evidence)**, потому что raw output и sanitized status не сохранены; исторические claimed verdicts не подтверждаются и не засчитываются. Прямая проверка raw исправила прежнюю сводку H2: на `a54fb9…` H2-r2 и H2-r3 — **FAIL**, а не 2 PASS; на `e618d93…` H2-r1, H2-r2 и H2-r3 — **FAIL**, H2-r4 — PASS. Все H2 FAIL подтверждены отсутствием `<финал>`/`</финал>` и неподтверждённой атрибуцией; H2-r1 также содержит постскриптум с альтернативой. На финальном для того аудита `924a688…` Mistral установлен и проверен после reload, однако H2-r1 заблокирован лимитом; Kimi current install и поведенческие проверки заблокированы capacity queue. PASS/FAIL на `924a688…` нет. Raw-файлы: `tests/runs/mistral-2026-11-25-r1/`, `tests/runs/kimi-2026-11-25-r2/`, `tests/runs/mistral-2026-11-25-r2/`; `H4-r1-before-fix.md` пуст из-за ошибки slice, дефект зафиксирован в status-файле.

### Внешние проверки

| Проверка | Результат | Условия |
| --- | --- | --- |
| skills.sh repo page | PASS | Доступна; 1 skill, 1 total install на 25.11.2026 |
| skills.sh skill page | STALE | Показывает старый 2.4.x body/description с разрешением unsourced additions |
| Snyk public report | FAIL (stale) | Audit от 23.11.2026: CRITICAL E004, MEDIUM W011, MEDIUM W012; не относится к новому SHA |
| Snyk rescan 2.5.0 | BLOCKED | Повторная публичная проверка 25.11.2026 20:06 UTC по-прежнему показывает audit от 23.11.2026 07:35 и старый snapshot с unsourced additions; нового SHA для rescan нет, API требует Vercel OIDC |
| skills.sh search `nachisto` | PASS | 25.11.2026 19:14 UTC: exact-name result found |
| skills.sh search `russian humanizer` | FAIL | Current snapshot absent from top-20 |
| skills.sh search `remove AI writing patterns russian` | FAIL | Current snapshot absent from top-5 |
| skills.sh search `очеловечить текст` | FAIL | Current snapshot absent from top-6 |
| skills.sh search `убрать машинность из текста` | FAIL | No skills found; stale-index observation |
| `npx -y skills@1.9.0 --help` | PASS | Команды `add`, `--global`, `--agent`, `--skill`, `--list`, `--copy` подтверждены выполнением |

Release gate: **CLOSED**. Offline scorer намеренно не открывает release gate без человеческого semantic review. На момент этого исторического аудита commit, push, GitHub About/Topics и публикация не выполнялись; позднее release candidate был опубликован отдельно.

## Исторические прогоны 2.4.x

| Дата | Платформа | Скилл | Кейс | Вердикт | Отклонение |
| --- | --- | --- | --- | --- | --- |
| 2026-11-22 | claude.ai web | 2.4.5 | 1 | PASS | — |
| 2026-11-22 | claude.ai web | 2.4.5 | 7 (генерация→правка, блог) | PASS | серая зона: утверждение исходника изменено до ответа автора (противоречие «первым делом» vs «если бы начали»), раскрыто вопросом; постскриптум после финала |
| 2026-11-22 | claude.ai web | 2.4.5 | 1 (повтор) | PASS | минор: неверные §-ссылки в «Первом переписе» (§12/§2 вместо §4/§13); постскриптум после финала |
| 2026-11-22 | claude.ai web | 2.4.5 | 2 | **FAIL** | канцелярит §10–§11 правлен по «очеловечь» вопреки исключению → хардендинг v2.4.6 |
| 2026-11-22 | claude.ai web | 2.4.5 | 7 (генерация→правка, маркетинг) | PASS | постскриптум после финала («Обещание про „две недели“…») → запрет v2.4.6 |
| 2026-11-22 | Mistral Vibe | 2.4.6 | Т1 (генерация) | PASS | скилл не сработал (негативный скоуп) |
| 2026-11-22 | Mistral Vibe | 2.4.6 | 7 (Т2, блог) | PASS | формат отчёта дословный, без постскриптума |
| 2026-11-22 | Mistral Vibe | 2.4.6 | 5 (молчаливая вставка) | PASS | диагностика с верными §4/§13/§1/§22 + вопрос; после подтверждения — строка «Потеряно» |
| 2026-11-22 | Mistral Vibe | 2.4.6 | 3 (неформальный) | PASS | злость сохранена, «сделал перезагрузку» → «три раза перезагрузил» |
| 2026-11-22 | Mistral Vibe | 2.4.6 | 2 (юридический) | PASS | §10–§11 заморожены, скилл процитировал хардендинг-строку; серая зона: чат-обёртка оставлена с обоснованием «вы просили её» → кейс 7 закрепляет: вырезается всегда |
| 2026-11-22 | Mistral Vibe | 2.4.6 | A5 (резюме) | PASS | скилл не сработал (негативный контроль удержался) |
| 2026-11-22 | Mistral Vibe | 2.4.8 | П5 (маркетинг) | PASS | обёртка убрана, §16 не применён, §6 обоснованы |
| 2026-11-22 | Mistral Vibe | 2.4.8 | П6 (перевод) | PASS | применён на явную просьбу, фактов не добавлено |
| 2026-11-22 | Mistral Vibe | 2.4.8 | П7 (стихи) | **FAIL** | художественное исключение не удержалось: рифма сломана без оговорки + постскриптум-вопрос → хардендинг v2.4.9 |
| 2026-11-22 | Mistral Vibe | 2.4.8 | A3 (EN-триггер) | **FAIL** | скилл сработал, § верные, но после финала добавлен постскриптум, запрещённый контрактом |
| 2026-11-22 | Mistral Vibe | 2.4.8 | Н1 (no-op) | PASS | текст возвращён без изменений, «штампов не нашла» |
| 2026-11-22 | Mistral Vibe | 2.4.8 | Н2 (добавление) | BLOCKED | не пройден: лимит бесплатного тарифа (сброс через 4ч) |
| 2026-11-22 | Mistral Vibe | 2.4.8 | Н4 (слабый порог) | BLOCKED | не пройден: лимит |
| 2026-11-22 | Mistral Vibe | 2.4.8 | Н5 (инъекция) | BLOCKED | не пройден: лимит |
