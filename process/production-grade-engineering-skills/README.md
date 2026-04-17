# Production Engineering Agent Skills

> 20 structured workflows that turn AI coding agents into disciplined senior engineers — from spec to ship.

**Pack slug:** `production-engineering-agent-skills`
**Version:** 1.0–1.2
**Author:** Addy Osmani (adapted from [`addyosmani/agent-skills`](https://github.com/addyosmani/agent-skills))
**Maintained by:** rob@workdynamite.com on ModelBound

---

## What it does

Loads 20 production-grade engineering skills into your AI coding agent so it follows the full software lifecycle instead of just generating code:

**Define → Plan → Build → Verify → Review → Ship**

Each skill is a self-contained workflow the agent can invoke or follow automatically. The pack covers:

- Spec-driven development & requirements capture
- Test-driven development (TDD) with realistic fixtures
- Incremental implementation (no 500-line first drafts)
- Code review checklists (correctness → security → readability → tests)
- Security hardening (input validation, authz, secrets handling)
- Performance optimization (profile-first, no premature opts)
- Git workflow (small commits, conventional commits, PR hygiene)
- CI/CD safety nets
- Observability & logging discipline
- Documentation as you go

## Who it's for

- **Engineering teams** standardizing how their AI agents ship code
- **Solo developers** who want their agent to behave like a senior teammate, not an intern
- **Platform/DevOps engineers** rolling out agent governance across multiple repos
- **Tech leads** tired of reviewing AI-generated PRs that skip tests or leak secrets

If you've ever had an agent ship a feature without writing a single test, this pack is for you.

## What's inside

20 skills, organized by lifecycle stage. The full list is in `SKILLS.md` inside the pack. Highlights:

| Stage | Example skills |
|---|---|
| **Define** | spec-driven-development, requirements-elicitation, acceptance-criteria |
| **Plan** | task-decomposition, architecture-decisions, risk-assessment |
| **Build** | tdd-workflow, incremental-implementation, refactoring-patterns |
| **Verify** | test-coverage, integration-testing, manual-qa-checklist |
| **Review** | code-review, security-review, performance-review |
| **Ship** | git-workflow, ci-cd-safety, release-notes, post-deploy-monitoring |

## Install

### Cursor (`.cursor/rules/`)

```bash
mkdir -p .cursor/rules
# Copy each skill as a .mdc file
```

Frontmatter for each rule:

```mdc
---
description: TDD workflow — write the failing test first, then the minimum code to pass.
globs: ["**/*.ts", "**/*.tsx", "**/*.py"]
alwaysApply: false
---
```

Use `alwaysApply: true` for foundational skills like `code-review` and `security-review`.

### Kiro (`.kiro/steering/`)

```bash
mkdir -p .kiro/steering
# Drop each skill as a .md file. Kiro loads all steering files globally.
```

For per-feature work, mirror the relevant skills into `.kiro/specs/<feature>/`.

### Windsurf (`.windsurfrules` or `.windsurf/rules/`)

Either:
- Concatenate all skills into a single `.windsurfrules` at repo root, OR
- Split into `.windsurf/rules/<skill>.md` (Windsurf will merge them).

### Claude Code (`.claude/skills/`)

This pack maps 1:1 onto the [Agent Skills open format](https://agentskills.io). Each skill becomes:

```
.claude/skills/<skill-name>/SKILL.md
```

With frontmatter:

```yaml
---
name: tdd-workflow
description: Write a failing test before any production code. Then write the minimum code to pass.
---
```

### GitHub Copilot (`.github/copilot-instructions.md`)

Concatenate the 5–6 skills you care most about into one file. Copilot weights the top of the file most heavily — put hard constraints first.

### Continue.dev (`.continue/rules/`)

One `.md` per skill, scoped with `globs:` frontmatter for language-specific rules.

## Usage tips

- **Don't load all 20 skills at once** in token-limited IDEs. Start with `tdd-workflow`, `code-review`, `git-workflow`, `security-review`. Add more as you see gaps.
- **Pin foundational skills** as `alwaysApply: true` (Cursor) or in steering (Kiro). Make per-language skills conditional via globs.
- **Override per project**: if your team doesn't use TDD, remove that skill rather than fighting it.
- **Pair with a project-specific `instructions.md`** that describes your stack — the skills handle *how to engineer*, your instructions handle *what this codebase is*.
- **Test the pack**: ask your agent to "add a new endpoint that does X" and verify it (1) writes a test first, (2) commits incrementally, (3) flags security concerns.

## Compatibility

| IDE / Agent | Status | Notes |
|---|---|---|
| Cursor | ✅ Recommended | Use globs to scope per-language skills |
| Kiro | ✅ Recommended | Maps cleanly to steering + specs |
| Windsurf | ✅ Supported | Use split rules for clarity |
| Claude Code | ✅ Native | Follows Agent Skills spec |
| GitHub Copilot | ⚠️ Partial | Single-file limit; pick 5–6 skills |
| Continue.dev | ✅ Supported | Scope with globs |
| Aider | ✅ Supported | Add via `--read CONVENTIONS.md` |
| Cline / Roo | ✅ Supported | Drop into `.clinerules` |

## Versioning

- **1.0** — Initial port of `addyosmani/agent-skills`
- **1.1** — Author attribution + minor cleanups
- **1.2** — Current. Tightened wording, removed duplicate guidance

Pull the latest via the ModelBound MCP server or clone from the marketplace.

## License & attribution

Original work © Addy Osmani — [`addyosmani/agent-skills`](https://github.com/addyosmani/agent-skills). Distributed here under the same terms as the upstream repo. ModelBound packaging and IDE-specific install instructions © ModelBound.

If you ship this in your team, please keep the upstream attribution.
