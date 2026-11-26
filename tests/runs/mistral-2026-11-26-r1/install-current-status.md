# Mistral current-payload install — qualified PASS

Date: 2026-11-26
Skill version: 2.5.0
Working-tree skill SHA-256: `06e843c7612b4237feee1d6e3c5c64713eefd71e5dbb872a8c9b1d73c8641be4`
Skill URL: https://chat.mistral.ai/skills/01a211da-1ec2-7663-aa6b-5a79e77cdab0
Model: not disclosed

Status: PASS for semantic save/reload through the standalone Mistral skill editor.

The exact generated `tests/runs/nachisto-current-payload.md` body was inserted into the existing enabled `nachisto` skill. Mistral displayed `Skill збережено`. After a fresh navigation/reload, the following checks were true:

- current hash header `06e843c7…`;
- exact 194-character description;
- skill enabled;
- source-only invariant;
- boundary chat-wrapper allowlist;
- repaired §1 verdict-preserving example;
- repaired §6 modality-preserving example;
- repaired §24 historical-action example.

Limitation: exact server-side byte identity is NOT VERIFIED. Mistral's Markdown editor/export normalizes formatting and escapes HTML markers, so literal marker extraction from an export is not a valid identity proof. This result proves semantic persistence of the current payload, not byte-for-byte storage or runtime behavior.
