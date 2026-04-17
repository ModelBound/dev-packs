# Senior Code Review

> Reviews PRs like a 10-year staff engineer: correctness → security → readability → tests, in that order.

**Pack slug:** `senior-code-review`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to do PR-quality code review — not "looks good 👍" review. Specifically:

1. **Correctness first**: does the code do what the diff claims? Edge cases? Null/empty/boundary?
2. **Security second**: input validation, authz checks, injection vectors, secret handling
3. **Readability third**: naming, function size, abstraction level, dead code
4. **Tests last (but required)**: are the new paths covered? Are the tests meaningful or `assert True`?

The agent writes review comments inline, prioritized by severity, with concrete suggested fixes — not vague "consider refactoring this."

## Who it's for

- **Engineering teams** using AI agents to pre-review PRs before human review
- **Solo developers** who want a second pair of eyes that doesn't get tired
- **Open source maintainers** drowning in drive-by PRs
- **Tech leads** who want consistency across reviewers

## What's inside

- `review-priorities.md` — the four-tier review order (correctness → security → readability → tests)
- `security-checklist.md` — OWASP-style checks tuned for app code
- `comment-style.md` — how to write actionable review comments
- `severity-labels.md` — `blocking` / `should-fix` / `nit` / `praise`

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Review code changes like a senior engineer. Correctness, security, readability, tests — in that order.
globs: ["**/*"]
alwaysApply: true
---
```

Set `alwaysApply: true` so it activates whenever you ask "review this."

### Kiro (`.kiro/steering/code-review.md`)

Add as steering — Kiro will use it whenever you ask for a review.

### Windsurf (`.windsurfrules`)

Append the review priorities to your root rules file.

### Claude Code (`.claude/skills/senior-code-review/SKILL.md`)

```yaml
---
name: senior-code-review
description: Review a code change or PR with senior engineer rigor. Use when the user asks to review, audit, or critique code.
---
```

### GitHub Copilot

Add the contents of `review-priorities.md` to `.github/copilot-instructions.md`. For inline PR review, also enable Copilot's PR review feature in repo settings.

### Aider

```bash
aider --read REVIEW.md <files-to-review>
```

## Usage tips

- **Give it the diff, not the whole repo.** Reviews are about *changes*, not the existing code.
- **Include the PR description** as context — the reviewer needs to know what the author claims to be doing.
- **Use severity labels** (`blocking`, `should-fix`, `nit`) so authors can triage. The pack teaches the agent to do this.
- **Pair with `security-hardening`** for security-sensitive code paths.
- **Don't auto-merge based on agent approval.** Use this as a first pass, not the final gate.

## Compatibility

| IDE / Agent | Status |
|---|---|
| Cursor | ✅ Recommended |
| Kiro | ✅ Recommended |
| Windsurf | ✅ Supported |
| Claude Code | ✅ Supported |
| Copilot | ✅ Supported (also via Copilot PR review) |
| Continue.dev | ✅ Supported |
| Aider | ✅ Supported |

## License

MIT — © ModelBound.
