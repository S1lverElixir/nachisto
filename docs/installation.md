[Installation](installation.md) | [Русский](installation.ru.md)

# Installation

nachisto is a directory-based Agent Skill. Install the whole directory, not only `SKILL.md`, because the editing workflow reads `references/patterns.md`.

The skill has no runtime dependencies. Git is required for clone and archive routes, Python 3.9 or later for the test harness, and Node.js 22.20.0 or later for the pinned Skills CLI.

## Choose a route

| Agent or surface | Recommended route | Evidence |
| --- | --- | --- |
| Any Agent Skills host | Clone into its user skill directory | Portable structure; host behavior varies |
| Claude Code | Marketplace plugin | Official plugin route; live 2.5.1 behavior not tested |
| Qoder CLI or IDE | Clone into `~/.qoder/skills/nachisto` | Official path; live 2.5.1 behavior not tested |
| Qoder app | Upload the six-file ZIP | Official upload route; live 2.5.1 behavior not tested |
| Qoder Cloud Agents | Upload the six-file ZIP through the Skills API | Official API route; live 2.5.1 behavior not tested |
| Cursor | Clone into `~/.cursor/skills/nachisto` | Official path; live 2.5.1 behavior not tested |
| claude.ai | Upload the six-file ZIP | Official custom-skill route; live 2.5.1 behavior not tested |
| MiniMax, Codex, pi, other CLI targets | Pinned `skills` CLI | Target documented by `skills@1.9.0`; host behavior not tested |
| ZCode | Clone into `~/.zcode/skills/nachisto` and refresh | Official path; live 2.5.1 behavior not tested |

See the [compatibility matrix](compatibility.md) for the meaning of these evidence levels.

## Universal clone

For agents that discover user skills in `~/.agents/skills/`:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/S1lverElixir/nachisto ~/.agents/skills/nachisto
```

The resulting layout must contain:

```text
~/.agents/skills/nachisto/
├── SKILL.md
├── references/
│   └── patterns.md
├── LICENSE
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
└── .cursor-plugin/
    └── plugin.json
```

Do not install into a directory that already contains unrelated files. For managed deployments, pin a reviewed release or commit instead of tracking a moving branch.

## Claude Code

Add this repository as a marketplace and install the plugin:

```text
/plugin marketplace add S1lverElixir/nachisto
/plugin install nachisto@nachisto
```

The marketplace is defined in `.claude-plugin/marketplace.json`. The plugin manifest points to the repository root with `skills: ["./"]`.

If `/plugin` is unavailable, update Claude Code and check the current [plugin documentation](https://code.claude.com/docs/en/plugins).

## Qoder

### CLI and IDE

Clone into Qoder's user skill directory:

```bash
mkdir -p ~/.qoder/skills
git clone https://github.com/S1lverElixir/nachisto ~/.qoder/skills/nachisto
```

Qoder CLI loads new skills at startup. In an existing session, run:

```text
/skills reload
/skills
```

Qoder IDE requires a restart. Type `/` in the chat and look for `nachisto`.

References: [Qoder CLI Skills](https://docs.qoder.com/cli/Skills), [Qoder IDE Skills](https://docs.qoder.com/extensions/skills).

### Qoder app

Build the ZIP described below, then open **Extensions → Skills → Add Skills → Upload Skill**. Upload one ZIP, `.skill`, or `SKILL.md` file at a time; folders cannot be uploaded directly. Use the ZIP so `references/patterns.md` is included.

Qoder also discovers skills in `~/.agents/skills`. After installation, confirm that `nachisto` appears under **Installed**, then invoke it with `/` and run a small test.

Reference: [Qoder Skills](https://docs.qoder.com/qoder/skills).

### Qoder Cloud Agents

Cloud Agents accepts a ZIP or multipart file tree with one top-level directory named `nachisto/`. Build the six-file archive described below, then upload it through the Cloud Agents Skills API:

```bash
curl -X POST https://api.qoder.com/api/v1/cloud/skills \
  -H "Authorization: Bearer $QODER_ACCESS_TOKEN" \
  -F "files=@nachisto.zip"
```

Bind the returned skill ID to an agent through the Cloud Agents API. Availability may require early access, and live 2.5.1 behavior has not been tested. The archive and uncompressed content must each be 50 MB or less.

Reference: [Qoder Cloud Agents Skills](https://docs.qoder.com/cloud-agents/skills).

## Cursor

Clone into Cursor's user skill directory:

```bash
mkdir -p ~/.cursor/skills
git clone https://github.com/S1lverElixir/nachisto ~/.cursor/skills/nachisto
```

Cursor discovers skills at startup. Open **Customize → Skills** or type `/nachisto` in Agent chat. Project-level installs can use `.agents/skills/nachisto/` or `.cursor/skills/nachisto/`.

The repository also contains `.cursor-plugin/plugin.json`. Cursor treats a root `SKILL.md` as a single-skill plugin when no `skills/` directory or manifest `skills` field exists. Local plugin behavior for 2.5.1 has not been live-tested.

References: [Cursor Skills](https://cursor.com/docs/skills), [Cursor Plugins](https://cursor.com/docs/reference/plugins).

## claude.ai

Create a ZIP with exactly one top-level `nachisto/` directory:

```bash
git clone https://github.com/S1lverElixir/nachisto nachisto
git -C nachisto archive --format=zip --prefix=nachisto/ -o ../nachisto.zip \
  HEAD SKILL.md LICENSE references/patterns.md \
  .claude-plugin/plugin.json .claude-plugin/marketplace.json .cursor-plugin/plugin.json
```

The archive must not contain the repository's documentation, tests, Git metadata, or a `nachisto-main/` wrapper. The folder name must match the skill name.

In claude.ai, upload the ZIP from **Customize → Skills → + → Create skill → Upload a skill**. Accounts on the unified rollout use **Plugins → +** instead; labels can vary. Skills/Plugins, code execution, and file creation must be enabled in account settings or through organization policy.

Standalone ZIP upload in Claude Desktop is not confirmed. A separate plugin-bundled route is documented for Claude Desktop and claude.ai; its live 2.5.1 behavior has not been tested here.

References: [Use skills in claude.ai](https://support.claude.com/en/articles/12512180-use-skills-in-claude), [Create custom skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills).

## Skills CLI

The installer version is pinned so the command does not change silently:

```bash
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent <agent>
```

Examples:

```bash
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent minimax-code
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent codex
npx -y skills@1.9.0 add S1lverElixir/nachisto --global --agent pi
```

The pinned target list is in [docs/agents.md](https://unpkg.com/skills@1.9.0/docs/agents.md). A documented CLI target proves an installation route, not live compatibility of nachisto with that host.

The CLI sends anonymous telemetry by default. Set `DISABLE_TELEMETRY=1` to opt out. See the [Skills CLI reference](https://www.skills.sh/docs/cli).

## ZCode

Clone into ZCode's user skill directory:

```bash
mkdir -p ~/.zcode/skills
git clone https://github.com/S1lverElixir/nachisto ~/.zcode/skills/nachisto
```

Open **Settings → Skills**, click **Refresh**, and confirm that the skill is enabled. ZCode documents `~/.zcode/skills/` and project-level `.zcode/skills/`; it also supports `.agents/skills`, but the global `~/.agents/skills/` route is not separately confirmed.

Reference: [ZCode Skills](https://zcode.z.ai/en/docs/skill).

## Manual copy

Copy the entire repository directory into the agent's user skill directory:

```bash
mkdir -p ~/.agents/skills
cp -R ./nachisto ~/.agents/skills/nachisto
```

Use this route only when the host documents that directory. Keep all runtime files together and do not overwrite unrelated content.

## Verify the installation

1. Confirm that the host lists `nachisto` and its description.
2. Use the host's manual invocation: `/nachisto` for standalone skills, `/nachisto:nachisto` for the Claude Code plugin, or `$nachisto` for ZCode.
3. Run a low-risk paste edit and check that the response includes the final block.
4. Confirm that a request to generate or translate text does not activate the humanizer workflow.
5. For file mode, confirm that the agent asks for separate write approval after showing a diff.

Do not treat successful discovery as proof of semantic quality. Live acceptance requires the frozen fixture matrix on the installed payload.

## Update

For a clone installation:

```bash
git -C ~/.agents/skills/nachisto pull --ff-only
```

Replace the path for Qoder, Cursor, or ZCode. Marketplace and CLI installations are updated by the host or by reinstalling the skill. The pinned CLI also provides `skills check` and `skills update`; check its current command reference before using them in automation.

Always verify the version in `SKILL.md` and the plugin manifests after an update. Automatic background updates are not promised.

## Managed deployment and Team/Enterprise

This is a public repository. It is not suitable for Claude Team/Enterprise **Sync from GitHub**, which requires a private or internal repository on github.com. Automatic sync also requires Cowork and Skills to be enabled, the Claude GitHub App to be installed, repository-admin access, an organization role of Owner/Primary Owner or an Enterprise custom role with library-management permission, and approved **Webhooks (Read & Write)** permission. When automatic sync is enabled, a pull request with a version bump that merges to the default branch triggers it; manual **Re-sync** is separate.

For managed distribution, use a reviewed internal copy, a pinned commit, or a release archive approved by the organization. Do not assume that every push updates every installed agent.

Reference: [Manage plugins for an organization](https://support.claude.com/en/articles/13837433-manage-plugins-for-your-organization).
