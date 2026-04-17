# ModelBound Dev Packs

> **Open, community-curated AI context for software engineering.**
> A collection of production-grade prompts, rules, skills, and memory files that make Cursor, Claude, Copilot, and Windsurf code like your best engineer — every time.

[![Powered by ModelBound](https://img.shields.io/badge/Powered%20by-ModelBound-6366f1)](https://modelbound.co)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](#-contributing)
[![Support](https://img.shields.io/badge/Support-support%40modelbound.co-blue)](mailto:support@modelbound.co)

---

## 📚 What is this repo?

This is the **official open-source home for ModelBound Dev Packs** — bundled, version-controlled AI context that teams use to standardize how their AI coding tools behave.

Each folder in this repo is a self-contained **Dev Pack** containing some combination of:

- **System prompts** — the role and constraints the AI runs under
- **Rules** — framework, style, and architecture conventions (Cursor, Copilot, Windsurf formats)
- **Skills** — reusable, task-specific instructions (refactor, review, write tests…)
- **Memory** — long-lived project knowledge and decisions
- **Evals** — test cases to verify the pack actually improves output

The packs in this repo are mirrored to the [**ModelBound Marketplace**](https://modelbound.co/marketplace), where they can be cloned into any workspace with one click.

---

## 📦 Available Dev Packs

Packs are organized by category. Each folder contains its own `README.md` describing what it does, what files it ships, and how to apply it.

### 🖼 Frameworks
| Pack | Use Case | Best For |
|------|----------|----------|
| `frameworks/perfect-react-refactor/` | Modern React refactoring, hooks, RSC | React / Next.js teams |
| `frameworks/nextjs-app-router-best-practices/` | Server components, caching, routing | Next.js 14+ |
| `frameworks/nodejs-backend-patterns/` | Express/Fastify, async patterns, errors | Node.js APIs |

### 🔤 Languages
| Pack | Use Case | Best For |
|------|----------|----------|
| `languages/typescript-strictness/` | Strict TS, no-`any` enforcement | TypeScript codebases |
| `languages/python-test-writer/` | Pytest-style test generation | Python projects |

### 🏛 Architecture
| Pack | Use Case | Best For |
|------|----------|----------|
| `architecture/clean-architecture-enforcer/` | Layered architecture & DDD | Backend services |
| `architecture/api-design/` | REST/GraphQL API design review | API teams |

### 🗄 Data
| Pack | Use Case | Best For |
|------|----------|----------|
| `data/sql-migration-reviewer/` | Safe migrations, indexing, locks | Postgres / MySQL |

### ✅ Quality
| Pack | Use Case | Best For |
|------|----------|----------|
| `quality/senior-code-review/` | Rigorous PR-style code review | Any codebase |
| `quality/tailwind-design-system-enforcer/` | Token-driven Tailwind | Design-system teams |

### ⚙️ Process
| Pack | Use Case | Best For |
|------|----------|----------|
| `process/production-grade-engineering-skills/` | Planning, debugging, CI/CD, and engineering workflows | Any team |

---

## 🚀 Quick Start

### Option 1 — Clone from the ModelBound Marketplace (recommended)

1. Sign up at [modelbound.co](https://modelbound.co) (free tier available)
2. Open the [Marketplace](https://modelbound.co/marketplace)
3. Find a pack and click **Clone to my workspace**
4. (Optional) Connect your repo and one-click sync the pack files into it

You get versioning, AI review, eval scoring, team sharing, and auto-optimization out of the box.

### Option 2 — Use the files directly from this repo

```bash
git clone https://github.com/modelbound/dev-packs.git
cp -r dev-packs/frameworks/perfect-react-refactor/. /path/to/your/project/
```

Then point your IDE at the files (see **IDE Setup** below).

### Option 3 — Pull packs on-demand via MCP (zero install)

If your agent supports the [Model Context Protocol](https://modelcontextprotocol.io) (Cursor, Claude Code, Claude Desktop, Windsurf, Codex, or any custom agent), you can skip cloning entirely and let the agent fetch packs from ModelBound at runtime.

```jsonc
// .cursor/mcp.json  (or claude_desktop_config.json)
{
  "mcpServers": {
    "modelbound": {
      "url": "https://mcp.modelbound.co",
      "headers": { "Authorization": "Bearer mb_live_YOUR_KEY" }
    }
  }
}
```

Then ask your agent:
> "Load the `senior-code-review` pack from ModelBound and review this PR."

The agent will call `get_pack`, pull the latest version, and apply it — no files in your repo, always up to date. See [MCP Server](#-mcp-server--use-packs-without-cloning) below.

---

## 🛠 IDE Setup

Dev Packs ship files that map cleanly to every major AI coding tool.

### Cursor
- `.cursor/rules/*.mdc` and `.cursorrules` are auto-detected
- `AGENTS.md` is loaded as agent-level context
- Reference any file with `@filename` in chat

### Claude Code / Claude Desktop
```bash
claude --system-prompt system-prompt.md
```
Or load the pack into a [Claude Project](https://claude.ai) as knowledge.

### GitHub Copilot
- Place `copilot-instructions.md` at `.github/copilot-instructions.md`
- Enable **custom instructions** in VS Code settings

### Windsurf
- `.windsurf/rules/*.md` are auto-loaded
- `AGENTS.md` provides directory-scoped context to Cascade

### Kiro
- `.kiro/steering/*.md` and `.kiro/specs/<feature>/{requirements,design,tasks}.md` are auto-loaded

### File reference (common across packs)

| File | Purpose |
|------|---------|
| `system-prompt.md` | Role, persona, and global constraints for the model |
| `.cursorrules` / `.cursor/rules/*.mdc` | Cursor-specific behavior rules |
| `copilot-instructions.md` | GitHub Copilot custom instructions |
| `.windsurf/rules/*.md` | Windsurf rule files |
| `AGENTS.md` | Cross-IDE agent context (Cursor, Windsurf, Kiro) |
| `skill-*.md` | Task-specific skill files (refactor, review, test…) |
| `memory.md` | Decisions, gotchas, project history |
| `evals/*.md` | Test cases verifying pack effectiveness |

---

## 🔁 How this repo connects to ModelBound

This isn't just a dump of static files — it's the public mirror of a living catalog.

```
   ┌─────────────────────────┐         ┌──────────────────────────┐
   │ ModelBound Marketplace  │ ◀─────▶ │   github.com/modelbound  │
   │  (clone, version, eval) │   sync  │       /dev-packs         │
   └─────────────────────────┘         └──────────────────────────┘
              ▲                                       ▲
              │ clone / publish                       │ PRs / issues
              ▼                                       ▼
       Your ModelBound workspace          The open-source community
              │
              │ one-click sync
              ▼
        Your Git repo + IDE
```

- **Publish:** any pack you build in ModelBound can be pushed to a GitHub repo
- **Mirror:** the official seed packs in this repo are published & maintained by ModelBound
- **Round-trip:** approved community PRs here flow back into the Marketplace so every cloner gets the improvement

---


## 🔌 MCP Server — use packs without cloning

ModelBound runs a hosted **Model Context Protocol** server at `https://mcp.modelbound.co` that exposes every dev pack (and your private library) as live tools your agent can call.

### Why use it?

| Problem with copying files | What MCP solves |
|---|---|
| Packs go stale the moment you clone | Always fetches the latest version |
| Have to re-clone for every project | One config, works everywhere |
| Can't search across packs from inside the agent | `search_all`, `get_pack`, `list_skills` tools |
| No telemetry on what worked | Runs are logged for eval & improvement |
| Manual updates when a pack improves | Updates flow automatically |

### Supported agents

- **Cursor** & **Windsurf** — native MCP via `.cursor/mcp.json`
- **Claude Code** & **Claude Desktop** — direct HTTP or `mcp-remote` proxy
- **Codex** and any agent harness that speaks JSON-RPC over HTTP
- **Custom agents** — full tool list at [mcp.modelbound.co/docs](https://modelbound.co/guides/serve-mcp-context)

### Tools relevant to dev packs

- `list_packs` / `get_pack` — browse and load any published pack
- `search_all` — semantic search across packs, rules, skills, and corpora
- `list_skills` / `get_skill` — pull individual skills (e.g. "write-pytest-tests")
- `add_file_to_pack` / `create_pack` — contribute back from inside your agent
- `optimize_tokens` / `estimate_conversation_cost` — keep context lean

### Getting an API key

1. Sign up at [modelbound.co](https://modelbound.co)
2. Go to **Settings → API Keys**
3. Generate a key with `Read` scope (or more if you want write access)
4. Drop it into your MCP client config

> 💡 **Read-only keys are safe to share with teammates** — they can consume packs but not modify your library.

Full setup guide: **[Serve Context to Any AI Agent via MCP →](https://modelbound.co/guides/serve-mcp-context)**

---

## 🤝 Contributing

We **want** community contributions. If you have a better React refactor prompt, a smarter SQL reviewer, or a brand-new pack for your favorite stack — open a PR.

### Contribute to an existing pack

1. Fork this repo
2. Edit files inside the pack folder (e.g. `frameworks/perfect-react-refactor/system-prompt.md`)
3. If your change is non-trivial, add or update an eval case in `evals/`
4. Open a PR with:
   - **What** you changed
   - **Why** it improves the pack (example before/after, eval results, etc.)
   - The pack name in the PR title — e.g. `[perfect-react-refactor] tighten hook rules`

### Propose a new Dev Pack

1. Create a new folder under the appropriate category, kebab-case (e.g. `frameworks/rust-axum-patterns/`)
2. Include at minimum:
   - `README.md` — what it does, who it's for
   - `system-prompt.md` — the core prompt
   - One or more rule/skill files
   - 2–3 eval cases under `evals/`
3. Open a PR tagged `new-pack`

### Review & merge flow

1. A maintainer reviews the PR (correctness, scope, safety, style)
2. ModelBound runs the pack's evals against the change
3. On merge:
   - The change lands here on `main`
   - The Marketplace pack version bumps automatically
   - Existing cloners see an "Update available" notification in their workspace

### What we look for

- ✅ Concrete, testable instructions (not vague aspirations)
- ✅ IDE-agnostic where possible; IDE-specific files clearly named
- ✅ No secrets, no proprietary code, no PII
- ✅ Plays well with the rest of the pack (no contradictions)

### Code of Conduct

Be kind. Assume good faith. We follow the [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).

---

## 🐛 Issues & Feature Requests

- **Bug in a pack?** Open an issue with the pack name and a reproduction
- **Want a new pack?** Open an issue with the `pack-request` label and describe the use case
- **Security concern?** Email [support@modelbound.co](mailto:support@modelbound.co) directly — please don't open a public issue

---

## 🆘 Support & Contact

- 📧 **Email:** [support@modelbound.co](mailto:support@modelbound.co)
- 🌐 **Website:** [modelbound.co](https://modelbound.co)
- 📚 **Docs & guides:** [modelbound.co/guides](https://modelbound.co/guides)
- 💬 **Community Q&A:** [modelbound.co/community](https://modelbound.co/community)
- 🛒 **Marketplace:** [modelbound.co/marketplace](https://modelbound.co/marketplace)

---

## 🔒 Privacy & Security

- Packs in this repo are 100% open-source and contain no customer data
- When you sync a pack via ModelBound, files only land in repos you explicitly authorize
- ModelBound stores credentials encrypted and only with the scopes you grant
- Full details: [Privacy Policy](https://modelbound.co/privacy) · [Terms](https://modelbound.co/terms)

---

## 📄 License

All packs in this repository are released under the **MIT License** unless otherwise stated inside an individual pack folder. Use them, fork them, ship them in your products — just don't claim you wrote the originals.

---

<p align="center">
  <b>Built and maintained by <a href="https://modelbound.co">ModelBound</a></b><br>
  <sub>The context engineering platform for AI-powered development teams.</sub><br>
  <br>
  <a href="https://modelbound.co">modelbound.co</a> · <a href="mailto:support@modelbound.co">support@modelbound.co</a>
</p>
