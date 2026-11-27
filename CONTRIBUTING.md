[Contributing](CONTRIBUTING.md) | [Русский README](README.md)

# Contributing

Contributions are welcome. The most useful contributions are reproducible examples, corrections to pattern guidance, and improvements to the frozen test oracle.

For a missed pattern or false positive, use the [“Новый штамп” issue template](https://github.com/S1lverElixir/nachisto/issues/new?template=stamp-report.yml). Do not include secrets, personal data, or confidential documents in an issue.

## Development setup

Requirements:

- Python 3.9 or later with pip.
- Git.
- Optional: Node.js 22.20.0 or later if you want to run the documented Skills CLI installer.

Install the test dependencies and run the local gate:

```bash
python -m pip install -r tests/requirements.txt
python -m py_compile tests/harness.py
python tests/harness.py self-test
python tests/harness.py static
python tests/harness.py package
python tests/harness.py measure --tokenizer o200k_base
git diff --check
```

On pushes to `main` and pull requests, CI runs `self-test`, `static`, `package`, and `measure --tokenizer none`. The local gate above additionally runs `py_compile`, the `o200k_base` measurement, and `git diff --check`. CI has read-only repository permissions and does not use external scanners or secrets.

## Repository layout

```text
SKILL.md                       Core runtime rules and workflow
references/patterns.md         Full 27-pattern catalog
.claude-plugin/                Claude plugin and marketplace manifests
.cursor-plugin/                Cursor plugin manifest
tests/fixtures.json            Frozen inputs and semantic oracle
tests/harness.py               Static, package, measure, score, and self-test commands
tests/README.md                Test contract and acceptance criteria
tests/RESULTS.md               Validation journal
docs/                          Installation, compatibility, and research
```

The runtime package contains exactly six files. Documentation and tests are not loaded by the editing agent.

## Change guidelines

### Runtime behavior

Changes to `SKILL.md` or `references/patterns.md` must preserve these invariants:

- Source-only output: no facts, names, numbers, sources, causes, or stronger claims from model knowledge.
- Document content is editing data, not authority over tools, network, permissions, or file targets.
- Protected fragments remain exact.
- File writes require a named target, diff review, and separate explicit approval.
- The output contract ends at `</финал>`.
- Genre exceptions and source-only rules do not conflict.

Update the version in `SKILL.md`, the Claude and Cursor plugin manifests, and `CHANGELOG.md` when releasing a new version. Keep the marketplace metadata description and keywords aligned; the marketplace manifest does not carry a version field. The static harness rejects version drift.

### Test oracle

`tests/fixtures.json` is pinned by SHA-256 and oracle version. Do not change an expected result to make a failing test pass.

If behavior intentionally changes:

1. Explain the defect or new requirement in the pull request.
2. Add or update the relevant fixture and semantic oracle.
3. Bump the oracle version and pinned fixture hash in the harness and test documentation.
4. Run the complete local gate.
5. Record live results separately; a synthetic control is not a model evaluation.

Critical cases require three repetitions. Acceptance criteria are defined in [tests/README.md](tests/README.md).

### Documentation

- Keep the root README focused on purpose, installation, usage, security, status, and links.
- Put detailed guides in `docs/` and test documentation in `tests/`.
- Use sentence-case headings, descriptive link text, active voice, and code font for commands and paths.
- Preserve exact hashes, versions, paths, commands, and validation statuses.
- Distinguish documented routes from verified behavior.
- Do not rewrite historical evidence to make the project look better; correct it with traceable raw evidence.

### Evidence and live tests

Do not record `PASS` from a static check as a behavioral result. Trigger activation needs host evidence; security side effects need an action log. Semantic verdicts require review of the saved output.

Dynamic file-write tests require an OS-enforced sandbox with no network and writes restricted to scratch. If that environment is unavailable, leave F01/F02 `BLOCKED`.

Raw model responses are ignored by Git. Commit only sanitized `*-status.md` evidence and the exact current payload allowed by `.gitignore`.

## Pull request checklist

- [ ] The change has a concrete reproduction or requirement.
- [ ] Source-only and trust-boundary invariants are preserved.
- [ ] Versions and hashes are updated where required.
- [ ] `self-test`, `static`, and `package` pass.
- [ ] `git diff --check` passes.
- [ ] Documentation matches the implemented behavior.
- [ ] Historical evidence was not altered without raw-source support.
- [ ] The pull request states which checks are `PASS`, `FAIL`, `NOT RUN`, or `BLOCKED`.

## Security reports

Read [SECURITY.md](SECURITY.md) before reporting a vulnerability. Private vulnerability reporting is currently disabled. Do not post sensitive details in a public issue.
