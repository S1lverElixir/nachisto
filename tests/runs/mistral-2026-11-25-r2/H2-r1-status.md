# Mistral H2-r1 — oracle-7 pre-contract-fix

Date: 2026-11-26
Status: FAIL
Skill version: 2.5.0
Skill SHA-256: `88242a31a8360cd71d058f9b373cea937457803f006a5370b5632a265a8efc29`
Oracle: `2.5.0-oracle-7`
Fixtures SHA-256: `30ad41d3a77169b35ac0da3177d06153e2a182e50c73ecacf653a52829dad5e1`
Platform: Mistral web, fresh skill-bound chat
Model: UI mode `Швидко`; exact model not disclosed
Chat: https://chat.mistral.ai/work/863d140b-2493-411a-bc31-f0a09f975504

The exact H2 fixture was submitted. The answer preserved `Орбита-7`, daily backups and reduced data-loss risk; it added no facts, included both final markers and wrote nothing after `</финал>`. FAIL is limited to the report contract: removal of an empty evaluation was classified as `Потеряно`, although §13 and the taxonomy classify it as `RHETORIC`. The answer began with `Первый перепис` and contained no visible reasoning. This result predates the output-contract fix and is not a test of the final `e618d93b…` payload.
