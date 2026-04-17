# Awesome ModelBound Dev Packs [![Awesome](https://awesome.re/badge.svg)](https://github.com/sindresorhus/awesome)

> Curated, production-grade AI context packs for developers who want their AI agents to write better code.

**Dev Packs** are modular, versioned collections of system prompts, project rules, and retrieval knowledge that turn generic AI coding assistants into specialized senior engineers.

---

## Contents

- [What are Dev Packs?](#what-are-dev-packs)
- [The Seed Packs](#the-seed-packs)
- [How to Use](#how-to-use)
  - [Option 1: One-Click Sync (Recommended)](#option-1-one-click-sync-recommended)
  - [Option 2: Clone & Customize](#option-2-clone--customize)
  - [Option 3: MCP Server (On-Demand)](#option-3-mcp-server-on-demand)
- [IDE & Editor Support](#ide--editor-support)
- [Creating Your Own Packs](#creating-your-own-packs)
- [Community & Contributing](#community--contributing)
- [Resources](#resources)

---

## What are Dev Packs?

Traditional AI coding assistants come with generic, one-size-fits-all prompts. **Dev Packs** change the game by providing:

- 🎯 **Specialized Context** — Each pack is tailored to a specific stack, pattern, or workflow  
- 🔄 **Versioned & Tested** — Packs include evaluation cases and performance metrics  
- 🌐 **Multi-Platform** — Works with Cursor, Claude Code, Windsurf, GitHub Copilot, and more  
- 🧩 **Composable** — Mix and match rules, skills, and memory files  
- 🤖 **MCP-Ready** — Access packs on-demand via the ModelBound MCP server

Think of them as **Docker images for AI context** — pre-configured environments that make your AI predictably excellent.

---

## The Seed Packs

This repository includes **10 production-grade starter packs**:

### 🚀 Framework-Specific

| Pack | Description | Best For |
|------|-------------|----------|
| **[perfect-react-refactor](./frameworks/perfect-react-refactor)** | React 18+ hooks, patterns, and performance optimization | Frontend teams |
| **[nextjs-app-router-best-practices](./frameworks/nextjs-app-router-best-practices)** | Next.js 14+ App Router, Server Components, caching | Full-stack Next.js |
| **[nodejs-backend-patterns](./frameworks/nodejs-backend-patterns)** | Express/Fastify patterns, middleware, error handling | API development |

### 🔤 Languages

| Pack | Description | Best For |
|------|-------------|----------|
| **[typescript-strictness](./languages/typescript-strictness)** | Advanced types, generics, strict mode patterns | Type-heavy codebases |
| **[python-test-writer](./languages/python-test-writer)** | Pytest-style test generation and best practices | Python projects |

### 🏛 Architecture & Design

| Pack | Description | Best For |
|------|-------------|----------|
| **[clean-architecture-enforcer](./architecture/clean-architecture-enforcer)** | Layered architecture, DDD, boundary enforcement | Backend services |
| **[api-design](./architecture/api-design)** | REST/GraphQL API design review and standards | API teams |

### 🗄 Data

| Pack | Description | Best For |
|------|-------------|----------|
| **[sql-migration-reviewer](./data/sql-migration-reviewer)** | Safe migrations, indexing, lock analysis | Postgres / MySQL |

### ✅ Quality

| Pack | Description | Best For |
|------|-------------|----------|
| **[senior-code-review](./quality/senior-code-review)** | Security, performance, maintainability analysis | PR reviews |
| **[tailwind-design-system-enforcer](./quality/tailwind-design-system-enforcer)** | Token-driven Tailwind design system enforcement | Design-system teams |

### ⚙️ Process

| Pack | Description | Best For |
|------|-------------|----------|
| **[production-grade-engineering-skills](./process/production-grade-engineering-skills)** | Planning, debugging, CI/CD, and engineering workflows | Any team |

---

## How to Use

### Option 1: One-Click Sync (Recommended)

Use the [ModelBound Dev Pack Marketplace](https://modelbound.co/marketplace) to sync packs directly into your team's library:

```
Browse → Select Pack → Sync to Team
```

Changes auto-sync when packs update. Perfect for teams.

### Option 2: Clone & Customize

```bash
git clone https://github.com/modelbound/dev-packs.git
cd dev-packs/frameworks/perfect-react-refactor
cp -r * ~/.cursor/rules/  # or your editor's context directory
```

Fork and customize for your organization's specific needs.

### Option 3: MCP Server (On-Demand)

Configure your agent to pull context on-demand via ModelBound's MCP server:

```json
{
  "mcpServers": {
    "modelbound": {
      "command": "npx",
      "args": ["-y", "@modelbound/mcp-server"],
      "env": {
        "MODELBOUND_API_KEY": "your-api-key"
      }
    }
  }
}
```

No cloning, no staleness — your agent always pulls the latest, battle-tested context.

---

## IDE & Editor Support

Dev Packs work with any AI-enabled editor:

| Editor | Integration Method | File Location |
|--------|-------------------|---------------|
| **Cursor** | `.cursorrules` + MCP | `.cursor/` |
| **Windsurf** | Cascade + Rules | `.windsurf/` |
| **Claude Code** | MCP Server | Config via `~/.claude/` |
| **GitHub Copilot** | Copy-paste or extension | `.github/copilot/` |
| **VS Code** | Custom Instructions | `.vscode/` |

---

## Creating Your Own Packs

Want to contribute a pack? Here's the structure:

```
my-awesome-pack/
├── README.md           # Pack description & usage
├── system-prompt.md    # Core persona instructions
├── rules/
│   ├── 01-architecture.md
│   ├── 02-coding-standards.md
│   └── 03-testing.md
├── skills/
│   └── specific-capabilities.md
├── memory/
│   └── domain-knowledge.md
└── evals/
    └── test-cases.json # Optional: evaluation suite
```

### Pack Standards

✅ **Single Responsibility** — One pack = one expertise area  
✅ **Tested** — Include eval cases if possible  
✅ **Versioned** — Semantic versioning for breaking changes  
✅ **Documented** — README explains when to use it  
✅ **Portable** — No hardcoded paths or secrets

---

## Community & Contributing

We ❤️ contributions! Here's how to get involved:

### 🔄 The Round-Trip Flow

1. **Fork & Clone** this repo
2. **Add or Improve** a pack
3. **Submit a Pull Request**
4. **We Review & Merge** → Updates sync to the Marketplace
5. **All Teams Benefit** from your improvements

### 📋 Contribution Checklist

- [ ] Pack follows the standard structure
- [ ] README explains the target use case
- [ ] No sensitive data or hardcoded credentials
- [ ] Eval cases included (optional but awesome)
- [ ] You've tested it with at least one AI assistant

### 💬 Get in Touch

- **Issues & Bugs**: [GitHub Issues](https://github.com/modelbound/dev-packs/issues)
- **Feature Requests**: [GitHub Discussions](https://github.com/modelbound/dev-packs/discussions)
- **Email**: [support@modelbound.co](mailto:support@modelbound.co)
- **Twitter**: [@modelbound](https://twitter.com/modelbound)

---

## Resources

### Official

- [🏠 ModelBound](https://modelbound.co) — The full context management platform
- [📦 Dev Pack Marketplace](https://modelbound.co/marketplace) — Browse & sync packs
- [📖 Documentation](https://modelbound.co/learning-center) — Guides & best practices
- [🔌 MCP Server](https://mcp.modelbound.co) — Real-time context access

### Related Awesome Lists

- [Awesome AI Agents](https://github.com/ai-agents/awesome-ai-agents)
- [Awesome Cursor](https://github.com/getcursor/awesome-cursor)
- [Awesome Claude](https://github.com/anthropics/awesome-claude)
- [Awesome MCP](https://github.com/modelcontextprotocol/awesome-mcp)

---

## License

MIT © [ModelBound](https://modelbound.co)

---

<p align="center">
  <sub>Built with ❤️ by the team at <a href="https://modelbound.co">ModelBound</a></sub>
</p>

<p align="center">
  <a href="https://modelbound.co">Website</a> •
  <a href="https://twitter.com/modelbound">Twitter</a> •
  <a href="mailto:support@modelbound.co">Contact</a>
</p>
