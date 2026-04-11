# claude-code-gh-repo-template

A project template for Claude Code that works out of the box — permissions configured, skills installed, agent rules auto-loaded.

## Quick Start

1. **Create a repo from this template** (GitHub → "Use this template" → "Create a new repository")
2. **Clone and open in Claude Code**
3. **One-time marketplace setup** (only needed once per machine):
   ```bash
   claude plugin marketplace add https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
   ```
4. **Run `/setup`** — Claude walks you through customizing for your project

Both plugins ([Superpowers](https://github.com/obra/superpowers) + [UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)) auto-install via `settings.json` once the marketplace is registered. Step 3 is only needed the first time on a new machine.

## What's Included

```
your-repo/
├── CLAUDE.md                          → project overview, committed
├── CLAUDE.local.md                    → personal overrides, gitignored
│
├── .claude/                           → the control center
│   ├── settings.json                     permissions + plugins, committed
│   ├── settings.local.json               personal overrides, gitignored
│   │
│   ├── commands/                      → custom slash commands
│   │   ├── setup.md                      /setup (interactive onboarding)
│   │   ├── review.md                     /review (self-review before commit)
│   │   ├── fix-issue.md                  /fix-issue (issue resolution)
│   │   └── deploy.md                     /deploy (deployment checklist)
│   │
│   ├── rules/                         → auto-loaded every session
│   │   ├── agent-behavior.md             dos/don'ts, safety boundaries
│   │   ├── code-quality.md               KISS mandate, code structure
│   │   ├── decision-making.md            when to ask vs decide
│   │   ├── communication.md              direct style, feedback
│   │   ├── git-workflow.md               worktrees, branch checks, PRs
│   │   ├── github-issues.md              issue-driven work, type prefixes
│   │   ├── linting.md                    lint tools by stack
│   │   ├── code-style.md                 placeholder — filled by /setup
│   │   ├── testing.md                    placeholder — filled by /setup
│   │   └── api-conventions.md            placeholder — filled or deleted by /setup
│   │
│   ├── skills/                        → project-specific skills (plugins auto-install)
│   │   └── .gitkeep
│   │
│   └── agents/                        → subagent personas
│       └── security-auditor.md           OWASP vulnerability scanning
│
├── .gitignore                         → ignores local files, docs/superpowers/
└── README.md                          → quick start, permissions, migration guide
```

**Plugins (auto-install via settings.json):**
- [`superpowers`](https://github.com/obra/superpowers) — TDD, debugging, planning, code review
- [`ui-ux-pro-max`](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) — design intelligence (requires one-time marketplace add)

## Customization

### Option 1: Run `/setup` (Recommended)

The setup command asks about your project and auto-configures everything:
- Fills in CLAUDE.md with your project details
- Configures code style and testing rules for your stack
- Trims linting rules to your relevant tools
- Updates permissions for your specific commands
- Cleans up unused placeholder files

### Option 2: Manual

1. Edit `CLAUDE.md` — fill in your project details
2. Edit `.claude/rules/code-style.md` — add your code conventions
3. Edit `.claude/rules/testing.md` — add your test commands
4. Edit `.claude/rules/api-conventions.md` — add API patterns (or delete if not an API)
5. Edit `.claude/commands/deploy.md` — add your deploy process
6. Edit `.claude/settings.json` — add project-specific commands to the allow list

## Permission Model

### Auto-Approved (No Prompts)

- All file reads, edits, and writes
- Test runners: pytest, jest, vitest, cargo test, go test
- Linters: eslint, prettier, black, ruff, shellcheck, tflint, golangci-lint
- Build tools: cargo build, go build
- Project scripts: `npm run *`
- Python: `python *.py`, `python -m *`
- Git: status, diff, log, branch, add, commit, checkout, stash, switch, pull, push, worktree
- GitHub CLI: `gh pr *`, `gh issue *`
- Directory listing: `ls`

### Blocked (Always Denied)

- `terraform *` — managed via CI/CD, not locally
- `git push origin main/master` — use feature branches
- `git push -f / --force` — prevents history rewriting
- `git worktree remove --force` — prevents discarding uncommitted work
- `rm -rf *` — prevents accidental deletion

### Requires Approval (Not in Allow or Deny)

- `npx *` — downloads arbitrary packages
- `make *` — executes arbitrary shell commands
- `python -c *` — arbitrary code execution

Add project-specific tools via `/setup` or manually in `.claude/settings.json`.

## Adding Your Own

### Rules
Create `.claude/rules/your-rule.md`. It auto-loads every session.
Add `paths:` frontmatter to scope it to specific files:
```yaml
---
paths:
  - "src/api/**/*.ts"
---
```

### Commands
Create `.claude/commands/your-command.md`. Invoke with `/your-command`.

### Agents
Create `.claude/agents/your-agent.md` with frontmatter:
```yaml
---
name: your-agent
description: "What this agent does"
tools:
  - Read
  - Grep
  - Glob
---
```

### Skills
Add to `.claude/skills/your-skill/SKILL.md` for on-demand workflows.

## Migrating from Root-Level Files

If you previously used AGENTS.md, SOUL.md, TEAM.md, PLANS.md at the repo root:

1. **Backup** any custom content you added to those files
2. **Copy** the `.claude/` directory from this template into your project
3. **Merge** your custom additions:
   - AGENTS.md customizations → `.claude/rules/agent-behavior.md` or a new rule file
   - SOUL.md customizations → `code-quality.md`, `decision-making.md`, or `communication.md`
   - TEAM.md customizations → create `.claude/rules/team-coordination.md`
4. **Update** your CLAUDE.md — remove `@AGENTS.md` / `@SOUL.md` imports
5. **Run** `/setup` to fill in placeholder rules
6. **Delete** the old root-level files
7. **Verify** — open Claude Code, confirm rules auto-load

Your `.claude/settings.local.json` and worktrees are unaffected.

## Philosophy

- **KISS above all** — simple solutions beat clever ones
- **Trust over bureaucracy** — clear guidelines, not micromanagement
- **Works out of the box** — clone and go, customize later
- **Single-agent focused** — no multi-agent complexity in the default setup

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Superpowers Plugin](https://github.com/obra/superpowers)
- [UI/UX Pro Max Skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
