# Mistral H2-r3 — final payload

Date: 2026-11-26
Status: FAIL
Skill version: 2.5.0
Skill SHA-256: `e618d93b710a00674a8a875a8ef47e00c43be49ff41889501742e471fd2f4b47`
Oracle: `2.5.0-oracle-7`
Fixtures SHA-256: `30ad41d3a77169b35ac0da3177d06153e2a182e50c73ecacf653a52829dad5e1`
Platform: Mistral web, fresh skill-bound chat
Model: UI mode `Швидко`; exact model not disclosed
Chat: https://chat.mistral.ai/work/7bfadbc9-436a-4011-afeb-b864c73b8ed3

The exact H2 fixture was submitted with the `nachisto` skill chip attached. The response preserved `Орбита-7`, daily backups and reduced data-loss risk and added no postscript. Semantic review: **FAIL**: the response omitted both required `<финал>` and `</финал>` markers and introduced unsourced attribution, «так пишут о нём». Direct inspection of the raw response disproves the earlier PASS claim. The `e618d93…` set is FAIL for H2-r1 (`H2-r1-finalhash-before-fix.md`), H2-r2 and H2-r3; only H2-r4 is PASS on that hash.
