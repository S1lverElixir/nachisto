# Mistral current-payload retry — browser transport

Date: 2026-11-26
Status: BLOCKED
Skill version: 2.5.0
Skill SHA-256: `590b844cba1292b02c56fa7029eb31eca626d797126f9beeb33817bc99bccaa0`
Oracle: `2.5.0-oracle-12`
Fixtures SHA-256: `65d18ad05837c47437993faba7a9011a0f877010d3b3ef395aaf9c9ac54bef16`
Platform: Mistral web, existing authenticated Chrome profile
Model: not selected; no behavioral request was sent

The exact current payload was regenerated locally at `tests/runs/nachisto-current-payload.md` (49,476 UTF-8 bytes), and literal extraction reproduced the advertised skill hash.

Retry observations:

- A fresh Browser Agent listed the connected Chrome instance successfully.
- Browser documentation loaded, but `browser.user.openTabs()` did not complete because the runtime reported `COMMAND_DRAINING: A previous tool is still cancelling. No new tool was dispatched.`
- A read-only retry returned the same draining state.
- Resetting the Node REPL discarded local bindings, but a fresh Browser Agent again reached the same server-side draining state before tab enumeration.
- No tab was claimed or created; no skill editor content was read or changed; no paste, save, reload, fixture submission, or model response occurred.

Current install/save/reload remains **BLOCKED** and current-hash behavioral cases remain **NOT RUN**. This retry does not inherit PASS from any historical payload. The earlier transport evidence for `c1479995…` remains in `tests/runs/mistral-2026-11-26-r2/browser-transport-status.md`.
