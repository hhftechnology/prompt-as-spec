# prompt-as-spec

A multi-agent skill and plugin repository for writing strict three-layer engineering specification prompts and enforcing architectural guardrails across:

- Discourse Ruby plugin development
- Discourse theme and theme component development
- Go backends and TypeScript/React frontends
- Astro 7 corporate websites

All instructions, templates, lint configs, and code comments are written in English.

## Repository Structure

```text
prompt-as-spec/
├── .claude-plugin/
│   └── marketplace.json                # Claude Code plugin marketplace catalog
├── plugins/
│   └── prompt-as-spec/
│       ├── .claude-plugin/
│       │   └── plugin.json             # Claude Code plugin manifest
│       └── skills/
│           └── prompt-as-spec/         # Skill payload for Claude Code
├── skills/
│   └── prompt-as-spec/                 # Cross-agent skill payload (Codex, Gemini CLI, skills.sh, Cursor)
│       ├── SKILL.md
│       ├── references/
│       │   ├── stack-profiles.md
│       │   ├── deterministic-gates.md
│       │   └── research-rationale.md
│       └── assets/
│           ├── system-spec-template.md
│           └── task-prompt-template.md
├── LICENSE
└── README.md
```

## Supported Platforms Overview

| Platform / Marketplace      | Discovery Path                         | Install Method                                                    |
| :-------------------------- | :------------------------------------- | :---------------------------------------------------------------- |
| Claude Code (Marketplace)   | `.claude-plugin/marketplace.json`      | `claude plugin marketplace add` + `claude plugin install`         |
| Universal (`skills.sh` CLI) | `skills/prompt-as-spec/SKILL.md`       | `npx skills add`                                                  |
| OpenAI Codex                | `.agents/skills/` or `~/.codex/skills/`| `npx skills add` or copy `skills/prompt-as-spec/`                 |
| Gemini CLI                  | `skills/prompt-as-spec/SKILL.md`       | `gemini skills install`                                           |
| Cursor / Windsurf / Copilot | `.agents/skills/` or `.cursor/skills/` | `npx skills add` or copy `skills/prompt-as-spec/`                 |

## Prerequisites for Private Repositories

Because this repository is **private**, ensure your local machine is authenticated with GitHub before running any remote installer:

```bash
# Authenticate with GitHub CLI (HTTPS) or ensure your SSH key is loaded
gh auth login
# Verify SSH access if using git@github.com URLs
ssh -T git@github.com
```

Replace `hhftechnology/prompt-as-spec` below if you host the repository under a different GitHub owner or name.

## Installation Methods

### 1. Claude Code (Plugin Marketplace)

Register this repository as a private plugin marketplace and install the plugin ([Claude Code Marketplace Docs](https://code.claude.com/docs/en/plugins/create-marketplace)):

```bash
# Terminal CLI
claude plugin marketplace add hhftechnology/prompt-as-spec
claude plugin install prompt-as-spec@hhf-skills
```

Or run inside an active Claude Code session:

```text
/plugin marketplace add hhftechnology/prompt-as-spec
/plugin install prompt-as-spec@hhf-skills
```

To verify the marketplace and plugin manifests locally before pushing updates:

```bash
claude plugin validate .
```

### 2. Universal Installer (`skills.sh` / `npx skills`)

Works across Claude Code, OpenAI Codex, Gemini CLI, Cursor, Windsurf, and other Agent Skills-compatible tools ([vercel-labs/skills](https://github.com/vercel-labs/skills)):

```bash
# HTTPS (requires gh / git credential helper for private repos)
npx skills add hhftechnology/prompt-as-spec --skill prompt-as-spec

# SSH (recommended for private repos)
npx skills add git@github.com:hhftechnology/prompt-as-spec.git --skill prompt-as-spec
```

### 3. OpenAI Codex

OpenAI Codex discovers skills in `.agents/skills/` (project-scoped) and `~/.codex/skills/` (user-scoped) ([OpenAI Codex Skills Docs](https://learn.chatgpt.com/docs/build-skills)).

Option A (via universal CLI):

```bash
npx skills add git@github.com:hhftechnology/prompt-as-spec.git --skill prompt-as-spec
```

Option B (manual clone into global Codex skills directory):

```bash
git clone git@github.com:hhftechnology/prompt-as-spec.git /tmp/prompt-as-spec
mkdir -p ~/.codex/skills
cp -r /tmp/prompt-as-spec/skills/prompt-as-spec ~/.codex/skills/
rm -rf /tmp/prompt-as-spec
```

### 4. Gemini CLI

Install directly from the repository path ([Gemini CLI Agent Skills Docs](https://geminicli.com/docs/cli/skills/)):

```bash
# User-scoped install via SSH
gemini skills install git@github.com:hhftechnology/prompt-as-spec.git --path skills/prompt-as-spec

# Workspace-scoped install from a local clone
git clone git@github.com:hhftechnology/prompt-as-spec.git /tmp/prompt-as-spec
gemini skills install /tmp/prompt-as-spec/skills/prompt-as-spec --scope workspace
```

### 5. Cursor, Windsurf, and GitHub Copilot

Use the shared `.agents/skills/` convention or copy directly into your editor's skills folder:

```bash
git clone git@github.com:hhftechnology/prompt-as-spec.git /tmp/prompt-as-spec
mkdir -p .agents/skills
cp -r /tmp/prompt-as-spec/skills/prompt-as-spec .agents/skills/
rm -rf /tmp/prompt-as-spec
```

## First-Time Push to Your New Private GitHub Repository

Unzip this repository archive locally, then initialize and push to your private repo:

```bash
cd prompt-as-spec-repo
git init -b main
git add .
git commit -m "Initial release: prompt-as-spec multi-marketplace skill repository"
git remote add origin git@github.com:hhftechnology/prompt-as-spec.git
git push -u origin main
```

## License

MIT
