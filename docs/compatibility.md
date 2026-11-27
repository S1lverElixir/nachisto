[Compatibility](compatibility.md) | [Русский](compatibility.ru.md)

# Compatibility and evidence

A documented installation route is not the same as verified runtime behavior. This matrix records what has actually been checked for nachisto 2.5.1.

## Evidence levels

| Level | Meaning |
| --- | --- |
| A | Live installation and behavior verified in a named version and environment. |
| B | Offline package smoke test passed: layout, contents, and extracted checks. |
| C | Official documentation plus static route or structure checks; live behavior not verified. |
| D | Not verified. |

A historical A for 2.4.x does not imply A for 2.5.x. Results apply to the exact payload hash and environment recorded in the validation journal.

## Current matrix

| Route or platform | Install/package evidence | Live 2.5.1 behavior |
| --- | --- | --- |
| Portable package | B | D — package smoke does not evaluate model output |
| Generic `~/.agents/skills/` clone | B — package structure | D |
| Claude Code marketplace | C — official docs and static manifests | D |
| claude.ai custom skill ZIP | C — official docs, ZIP layout, description limit | D — excluded from this live-test cycle by user choice |
| Claude Desktop custom skill upload | D — standalone upload not confirmed | D |
| Claude Desktop plugin-bundled skill | C — official plugin docs | D |
| Cursor standalone skill | C — official docs and user path | D |
| Cursor local plugin | C — root single-skill plugin structure | D |
| Qoder CLI and IDE | C — official paths and reload behavior | D |
| Qoder app upload | C — official ZIP/`.skill`/`SKILL.md` route | D |
| Qoder Cloud Agents package | C — official package route | D |
| MiniMax Code through Skills CLI | C — target listed by `skills@1.9.0` | D |
| ZCode | C — official path and refresh action | D |
| Codex and pi through Skills CLI | C — targets listed by `skills@1.9.0` | D |
| Mistral web skill editor and Vibe | D / NOT RUN for current payload | D / NOT RUN |
| Kimi web | D / NOT RUN — authentication not verified | D / NOT RUN |
| Other agents | D — target- and host-dependent | D |
| Claude Team/Enterprise GitHub sync | D — public repository is unsuitable | D |

## Current blockers

### Mistral

The current skill hash is `8630ce583d0c2063e117407a9fbf067e6877d942a68e441991f27ffa5a040eda`. The payload was regenerated and its literal install markers reproduce that hash.

The current 2.5.1 payload has not been submitted for install/save/reload or the fixture matrix. A historical 2.5.0 attempt was blocked by browser transport failures, including `COMMAND_DRAINING`; no replacement or save was confirmed. Historical install evidence does not grade the current payload.

Historical evidence: [browser transport status](../tests/runs/mistral-2026-11-26-r3/browser-transport-status.md).

### Kimi

Authentication and capacity for the current payload were not verified. The current payload was not submitted, so there is no current install or behavioral verdict.

Historical observation: [Kimi status](../tests/runs/kimi-2026-11-26-r1/current-status.md) records payload `fae10717…`, not `8630ce…`. It does not test the current hash.

### Dynamic file mode

F01/F02 require an OS-enforced sandbox with no network and writes restricted to scratch. That environment is not available, so the tests remain **BLOCKED**. The working repository and user files are not used as a test range.

### External scans and directory indexes

The public Snyk page still shows the audit from 2026-11-23 and an older skill snapshot. The current SHA has not been rescanned. The skills.sh index was stale during the audit; publication of the release candidate does not by itself prove that the index or scan has refreshed.

## What this matrix does not claim

- It does not claim that every listed platform has been tested live.
- It does not convert a documented CLI target into host compatibility.
- It does not treat historical payloads as evidence for the current hash.
- It does not treat offline evidence integrity as semantic review.
- It does not authorize a stable release while live acceptance remains incomplete.

The release acceptance gate remains **CLOSED**. See [tests/RESULTS.md](../tests/RESULTS.md) for the run journal and [tests/README.md](../tests/README.md) for acceptance criteria.
