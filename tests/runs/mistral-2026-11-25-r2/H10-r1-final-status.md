# Mistral H10-r1 — pre-fact-question payload

Date: 2026-11-26
Status: FAIL
Skill version: 2.5.0
Skill SHA-256: `e618d93b710a00674a8a875a8ef47e00c43be49ff41889501742e471fd2f4b47`
Oracle: `2.5.0-oracle-7`
Platform: Mistral web, fresh skill-bound chat
Model: UI mode `Швидко`; exact model not disclosed
Chat: https://chat.mistral.ai/work/1c8dbe7e-5f99-4711-8485-064fd082b769

The model preserved the false source date 1994 and the organization name, removed only the frame/chat closer, added no facts, and produced a valid final. FAIL: it did not ask the author whether the potentially false date should be checked or corrected separately. The source-only instruction now makes that question mandatory when a possible factual error is noticed.
