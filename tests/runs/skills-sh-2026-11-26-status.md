# skills.sh recheck — stale 2.4.x snapshot

Date: 2026-11-26
Status: BLOCKED
Checked payload at the time: working-tree 2.5.0, skill SHA-256 `3e80288ad05d963d0b6c0d26aca2656b950b1b7fc21a17c0ceb8edae3a48c8e6`. Later working-tree payloads `5747d03…`, `06e843c7612b4237feee1d6e3c5c64713eefd71e5dbb872a8c9b1d73c8641be4`, `c1479995bea9b6e4d7cbaad9da2b7176532176e3b67771bad485012dda5ae02c`, released `590b844cba1292b02c56fa7029eb31eca626d797126f9beeb33817bc99bccaa0`, and current 2.5.1 `8630ce583d0c2063e117407a9fbf067e6877d942a68e441991f27ffa5a040eda` have not been scanned or indexed.

## Public pages

- Repository page: PASS — reachable; one skill and one total install.
- Skill page: STALE — still displays the older 2.4.x body/description, including permission to add details from model knowledge. The working-tree 2.5.0 payload is not indexed.
- Snyk page: FAIL (stale) — rechecked 2026-11-26 01:16 UTC; audit remains Nov 23, 2026, 07:35 AM; CRITICAL E004, MEDIUM W011, MEDIUM W012. No rescan of working-tree SHA `590b844c…` is available. Missing current evidence is not PASS.
- Audit API: BLOCKED — current documentation requires Vercel OIDC; no authenticated request was made.

## Browser search observations

| Query | Observed result |
| --- | --- |
| `nachisto` | rank 1 |
| `убрать ии штампы` | rank 1 |
| `убрать машинность из текста` | rank 1 |
| `russian humanizer` | absent from returned top 20 |
| `humanize russian text` | absent from returned top 12 |
| `remove AI writing patterns russian` | absent from returned top 5 |
| `очеловечить текст` | absent from returned top 6 |
| `русский humanizer` | absent from returned top 5 |

Search footer: results are ranked by relevance, publisher, and installs. These observations describe the stale public index at the time of the check, not the current working tree.

Sources:
- https://www.skills.sh/s1lverelixir/nachisto
- https://www.skills.sh/s1lverelixir/nachisto/nachisto
- https://www.skills.sh/s1lverelixir/nachisto/nachisto/security/snyk
- https://www.skills.sh/docs/api
