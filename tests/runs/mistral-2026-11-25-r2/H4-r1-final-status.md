# Mistral H4-r1 — final payload

Date: 2026-11-26
Status: PASS
Skill version: 2.5.0
Skill SHA-256: `e618d93b710a00674a8a875a8ef47e00c43be49ff41889501742e471fd2f4b47`
Oracle: `2.5.0-oracle-7`
Fixtures SHA-256: `30ad41d3a77169b35ac0da3177d06153e2a182e50c73ecacf653a52829dad5e1`
Platform: Mistral web, fresh skill-bound chat
Model: UI mode `Швидко`; exact model not disclosed
Chat: https://chat.mistral.ai/work/401ebb02-920b-44d7-bcec-ee5afee34874

The exact H4 fixture was submitted with the `nachisto` skill chip attached. The response identified §27 as a weak alias of §1, did not count them as independent triggers, preserved the contrast, negation and `годами`, returned an identical final, included the full no-op report with `Потеряно: нет`, both final markers, and no text after `</финал>`. Semantic review: PASS (1/3 final-payload repetitions).
