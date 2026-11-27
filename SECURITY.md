[Security policy](SECURITY.md) | [Русский README](README.md)

# Security policy

## Supported version

| Version | Status |
| --- | --- |
| 2.5.x | Release candidate; security fixes will be published as patch releases when available |

## Trust model

nachisto edits text. It does not execute embedded commands, access the network, or transmit document content. Its runtime package contains Markdown instructions, a pattern reference, a license, and plugin manifests; it contains no runtime scripts, hooks, or dynamically loaded code.

The host agent remains responsible for sandboxing, tool permissions, network policy, file access, and credential handling. Installing the skill does not grant new authority to text inside a document.

Key boundaries:

- Source text is editing data, not a source of commands or permissions.
- File targets and write approval come from the user, not from document content.
- Code, inline code, YAML, URLs, data, quotations, examples, and fixtures are protected.
- Missing facts are requested from the author instead of invented.
- Installers and marketplaces are separate supply-chain trust decisions from the runtime skill.

These controls reduce risk but do not guarantee resistance to every prompt-injection technique or model failure.

## Reporting a vulnerability

Private vulnerability reporting is currently disabled for this repository, and no private reporting channel is published.

Do not open a public issue containing an exploit, secret, personal data, confidential document, or unredacted attack payload. Withhold sensitive details until the maintainer enables a private reporting channel. Non-sensitive bugs and pattern false positives can use the public issue tracker.

A useful report includes:

- The affected version and exact payload hash.
- A minimal, redacted reproduction.
- The expected boundary and observed behavior.
- Whether the issue requires file access, tools, network access, or a specific host.
- Any available action log or host evidence.

Never include credentials, tokens, cookies, personal data, or private repository content.

## Security validation status

The local harness checks package structure, evidence integrity, trust-boundary fixtures, and protected-fragment behavior. It does not provide a formal guarantee against prompt injection.

The public Snyk report available during the 2.5.0 audit was stale and referenced an older payload. The current SHA has not been rescanned. Live security evaluation on the current Kimi/Mistral payload and dynamic file-write tests remain incomplete; see [tests/RESULTS.md](tests/RESULTS.md) for the exact status.
