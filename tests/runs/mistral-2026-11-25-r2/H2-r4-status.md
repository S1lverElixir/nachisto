# Mistral H2-r4 — final payload

Date: 2026-11-26
Status: PASS
Skill version: 2.5.0
Skill SHA-256: `e618d93b710a00674a8a875a8ef47e00c43be49ff41889501742e471fd2f4b47`
Oracle: `2.5.0-oracle-7`
Fixtures SHA-256: `30ad41d3a77169b35ac0da3177d06153e2a182e50c73ecacf653a52829dad5e1`
Platform: Mistral web, fresh skill-bound chat
Model: UI mode `Швидко`; exact model not disclosed
Chat: https://chat.mistral.ai/work/fdc5fc1d-4a42-4ac6-8cbf-4f24093a2f07

The exact H2 fixture was submitted with the `nachisto` skill chip attached. The response began with `Первый перепис`, reported `Потеряно: нет`, preserved `Орбита-7`, daily backups and reduced data-loss risk, added no facts, included `<финал>` and `</финал>`, and had no text after the closing marker. Semantic review: PASS. Direct raw-output review corrected the earlier summary: H2-r1, H2-r2 and H2-r3 are FAIL, so this payload's observed H2 set is 1/3 PASS, not 3/3. See their raw files and `tests/RESULTS.md`.
