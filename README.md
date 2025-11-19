# Claude Code Configuration

My personal Claude Code setup as a medical doctor learning to build software.

## Context

I'm a GP transitioning out of clinical practice by building SaaS products and content businesses. Zero coding background before the AI era - feeling like an outsider in developer spaces but excited to learn.

## Why This Repo Exists

Three reasons:

1. **Contribute** - Maybe someone else finds these agents/workflows useful
2. **Learn from feedback** - I don't know what I don't know
3. **Self-help** - Ironically, putting this on GitHub means Claude can actually access and help me improve it

## What's Here

### Global Configuration
- `CLAUDE.md` - Development philosophy, git rules, Next.js patterns
- `settings.json` - Permissions and MCP server configuration

### Agents (7 specialists)
- `code-reviewer` - Quality checks, runs build/lint first
- `codebase-scanner` - Explores code, saves findings to files
- `implementation-planner` - Plans strategy using sequential thinking
- `failure-analyst` - Diagnoses test failures
- `documentation-maintainer` - Keeps docs lean and organized
- `progress-update` - Records session history
- `test-validator` - Validates test execution (WIP)

### Commands (5 workflows)
- `/commit` - Conventional commits
- `/deploy-check` - Pre-deployment checklist
- `/epc` - Explore → Plan → Code → Commit (default workflow)
- `/tdd` - Test-driven development (for risky changes)
- `/handover` - Save progress for context recovery

### Documentation
- `tailwind4guide.md` - Tailwind CSS 4 patterns
- `design-tokens.md` - Token philosophy
- `catalyst_master.md` - Catalyst UI components
- `MCP-Server-Configuration-Guide.md` - MCP setup

## My Approach

**Ship fast, iterate based on reality, not perfection.**

I'm treating Claude Code like a brilliant intern - capable but needs oversight. These agents are designed to:
- Fight my perfectionism
- Break down overwhelming projects
- Maintain quality despite my ADHD
- Keep things simple and boring (these are real businesses, not tech demos)

## Stack

Next.js 15 (App Router), React, Tailwind CSS 4, TypeScript, Vercel, Supabase

## Learning in Public

This is a work in progress. I'm figuring this out as I go. If you see something wrong or inefficient, please open an issue - I'd genuinely appreciate the feedback.

## Setup
```bash
# Clone to temporary location
git clone https://github.com/drdanmaggs/claude-config.git ~/claude-config-repo

# Symlink to ~/.claude/ (keeps version control separate from runtime files)
ln -sf ~/claude-config-repo/CLAUDE.md ~/.claude/CLAUDE.md
ln -sf ~/claude-config-repo/agents ~/.claude/agents
ln -sf ~/claude-config-repo/commands ~/.claude/commands
ln -sf ~/claude-config-repo/docs ~/.claude/docs

# Your settings.json stays local (contains API keys, not in git)
```

## License

MIT - Use however you like

---

**Status:** Active development, learning as I go

**Last updated:** November 2024
