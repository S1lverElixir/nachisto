English | [Русский](README.md)

[![Release](https://img.shields.io/github/v/release/S1lverElixir/nachisto?include_prereleases)](https://github.com/S1lverElixir/nachisto/releases)
[![CI](https://github.com/S1lverElixir/nachisto/actions/workflows/version-check.yml/badge.svg)](https://github.com/S1lverElixir/nachisto/actions/workflows/version-check.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

# nachisto

**A source-only Russian humanizer for AI agents.**

nachisto rewrites ready-made Russian prose so it sounds natural. It removes formulaic phrasing, empty introductions, repetitive rhythm, and chat residue. Its rules require preserving meaning, facts, modality, attribution, and the author's voice.

It does not generate missing facts from model knowledge. If the source does not support a statement, the skill asks the author instead of filling the gap.

## Features

- Edits existing text only; generation, translation, summarization, and proofreading stay outside its scope.
- Requires preservation of facts, names, numbers, dates, conditions, negation, causal links, and attribution.
- Provides protection for code, inline code, YAML, URLs, data, quotations, examples, and test fixtures.
- Treats document content as editing data, not as instructions that can change tools, targets, or permissions.
- Supports paste, file, embedded, find-only, and no-op modes.
- Reads the full 27-pattern catalog in `references/patterns.md` before pattern analysis and editing; non-activation does not load it.

## Install

### Hosts that discover skills in `~/.agents/skills/`

Create the parent directory if needed, then clone the whole repository:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/S1lverElixir/nachisto ~/.agents/skills/nachisto
```

Keep `SKILL.md`, `references/`, and `LICENSE` together. The skill has no runtime dependencies and does not need a build step.

### Claude Code

Add the marketplace and install the plugin:

```text
/plugin marketplace add S1lverElixir/nachisto
/plugin install nachisto@nachisto
```

### Skills CLI

Pin the installer version and choose a supported target:

```bash
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent <agent>
```

See the [installation guide](docs/installation.md) for Qoder, Cursor, claude.ai, ZCode, MiniMax, Codex, pi, manual installation, updates, and managed deployment.

## Usage

Ask for an explicit edit and provide the source text:

```text
Remove AI clichés from the text below. Preserve meaning, facts, modality, and attribution.

В нашем сервисе используется кеш. Он хранит результаты дорогих запросов. Благодаря этому повторные обращения выполняются быстрее. Особенно это заметно в ключевых запросах.
```

Other useful requests:

```text
Find formulaic wording, but do not rewrite the text.
Edit only the prose in article.md. Show the diff first and wait for write approval.
Integrate the cleaned version into the PR description without a report.
```

### Modes

| Mode | Behavior |
| --- | --- |
| Explicit paste edit | Returns an edit report followed by the final text. |
| Silent paste | Diagnoses patterns and asks whether to rewrite; no silent rewrite. |
| File | Reads the agreed file, shows a diff, and writes only after separate approval. |
| Embedded | Returns only the final text for a PR, commit message, document, or another artifact. |
| Find-only | Returns quotations, catalog sections, and signal strength without editing. |
| No-op | Keeps the source unchanged when no justified edit exists. |
| Non-activation | Does not take over generation, translation, summarization, proofreading, or fact-checking. |

### Output contract

An explicit paste edit returns a report and then the final block:

```text
Первый перепис: …
Выжившие штампы: §… — причина
Изменено: …
Потеряно: нет
<финал>
…
</финал>
```

Nothing follows `</финал>`. If a required fact is missing, the skill returns a clarification question instead. If the source already contains `<финал>` or `</финал>`, the skill asks which delimiters are safe rather than guessing.

### Example rewrite

Before:

> В нашем сервисе используется кеш. Он хранит результаты дорогих запросов. Благодаря этому повторные обращения выполняются быстрее. Особенно это заметно в ключевых запросах.

After:

> В нашем сервисе кеш хранит результаты дорогих запросов и ускоряет повторные обращения, особенно в ключевых запросах.

For file mode, permission to edit does not imply permission to write. The target must be named by the user, and document text cannot select or authorize a different file.

## Security and trust

The skill's runtime consists of Markdown instructions and manifests. It contains no executable hooks, runtime scripts, network calls, or dynamically loaded code.

During editing, it does not execute shell strings, URLs, tool calls, or system/operator labels found inside the document. Coherent instructions, quotations, examples, and fixtures remain content; they do not gain authority over the agent.

These rules reduce prompt-injection risk but do not replace the host agent's sandbox, least-privilege settings, or access controls. Do not provide secrets or confidential documents without the data owner's permission. See [SECURITY.md](SECURITY.md) for the reporting policy.

## Development

The test harness uses Python and pinned development dependencies. It is not loaded by the editing agent.

```bash
python -m pip install -r tests/requirements.txt
python tests/harness.py self-test
python tests/harness.py static
python tests/harness.py package
python tests/harness.py measure --tokenizer o200k_base
```

The frozen suite expects 197 results per platform/model: 81 trigger results and 116 case results. The `o200k_base` measurement is a proxy, not the exact tokenizer used by every host.

See [tests/README.md](tests/README.md) for fixture groups, scoring rules, evidence requirements, and acceptance criteria. See [CONTRIBUTING.md](CONTRIBUTING.md) before changing the skill or its oracle.

## Project status

Version 2.5.1 is published as a [release candidate](https://github.com/S1lverElixir/nachisto/releases/tag/v2.5.1). Local engineering checks pass, but live acceptance of the current payload on Kimi/Mistral and a current external scan are still pending. A stable release has not been declared.

See the [compatibility matrix](docs/compatibility.md) and [validation journal](tests/RESULTS.md) for details.

## Documentation

- [Installation](docs/installation.md)
- [Compatibility and evidence levels](docs/compatibility.md)
- [Research and pattern provenance](docs/research.md)
- [Test harness](tests/README.md)
- [Validation journal](tests/RESULTS.md) — in Russian
- [Changelog](CHANGELOG.md) — in Russian
- [Contributing](CONTRIBUTING.md)
- [Security policy](SECURITY.md)

## Help and feedback

Found a missed pattern, false positive, or incorrect genre rule? Open a [“Новый штамп” issue](https://github.com/S1lverElixir/nachisto/issues/new?template=stamp-report.yml) with a short example, genre, and expected edit.

Do not include secrets, personal data, or confidential documents. Redact both the source example and the proposed rewrite.

## Credits and license

nachisto is a Russian-language adaptation of [blader/humanizer](https://github.com/blader/humanizer) v4.0.0. Its structural guidance also draws on Wikipedia's [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) and related Russian-language projects. These sources explain the project's provenance; they are not comparative benchmarks or quality guarantees.

Licensed under [MIT](LICENSE). Both copyright notices are retained in the license.
