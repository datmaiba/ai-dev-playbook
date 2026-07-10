# AI Dev Playbook

> My personal system for working with AI coding agents (Claude Code, Cursor, GitHub Copilot) — portable across projects and stacks. Built from 9+ years of fullstack development and refined in production while running the AI-assisted modernization of a 10+ year-old legacy platform.

## The idea

AI agents write code fast, but on any long-running project **consistency is the bottleneck, not speed**. Every generation is a new "developer" with no memory: naming drifts, patterns get reinvented, silent bugs pass every automated check. The fix isn't better prompts — it's a *system*: written rules, pre-coding checks, and a feedback loop that turns every mistake into a permanent lesson.

This playbook is that system, split into three portable layers:

```
core/       →  how I work with AI on ANY project (stack-agnostic)
stacks/     →  pluggable rule profiles per stack (React+TS, PHP/Laravel)
templates/  →  drop-in starters to set up a NEW project in under an hour
```

When I join a project, I: copy a template, plug in the matching stack profiles, adapt the placeholders, and start the lessons-learned log. Day one, the agent works like it has been on the team for a year.

## Quick start

1. Follow [`templates/new-project-setup.md`](templates/new-project-setup.md) — a 5-step, ~1 hour checklist.
2. Copy [`templates/CLAUDE.template.md`](templates/CLAUDE.template.md) into your repo as `CLAUDE.md`, fill the placeholders, prune the matching [`stacks/`](stacks/) profile into it.
3. Type **"verify rules"** in a fresh agent session — if the agent can recite the rules, the system is live.

Skeptical it matters? Read [`examples/`](examples/) — the same prompt with and without rules, in both stacks.

## Repo map

| Path | What it is |
|------|------------|
| `core/00-philosophy.md` | The six principles everything else derives from |
| `core/01-gates-and-scope.md` | Non-bypassable gates, scope control, plan-gated execution |
| `core/02-feedback-loop.md` | lessons-learned → rule → linter: how rules stay alive |
| `core/03-environment.md` | One sanctioned toolchain path; why agents need it |
| `stacks/react-typescript.md` | React 18 + TS profile: architecture, tokens, silent-failure traps |
| `stacks/php-laravel.md` | PHP/Laravel profile: layered architecture, CQRS-lite services, legacy rules |
| `templates/CLAUDE.template.md` | Fill-in-the-blanks agent entrypoint for a new project |
| `templates/cursorrules.template` | Same rule set condensed for Cursor |
| `templates/lessons-learned.template.md` | Starter log with entry format |
| `templates/new-project-setup.md` | Day-1 checklist: wiring the system into a fresh repo |
| `skills/pre-coding-guardrails/SKILL.md` | The 9-guardrail checklist agents run before writing code |
| `checklists/ai-code-review-checklist.md` | 2-minute human review pass for AI-generated code |
| `lessons-learned/lessons-learned.md` | My own log (sanitized examples) |
| `examples/before-after-data-table.md` | Same prompt with vs. without rules — React + TS |
| `examples/before-after-laravel-service.md` | Same prompt with vs. without rules — PHP/Laravel |

## The six principles

1. **Rules are documents, not vibes.** Numbered markdown files with RFC 2119 keywords. "Please prefer X" gets ignored under context pressure; "MUST use X; violations are blocking defects" does not.
2. **One entrypoint per agent, one source of truth.** `CLAUDE.md` / `.cursorrules` point at the rules and quote only the most-violated ones — they never fork the content.
3. **Name the traps explicitly.** The highest-value rules document *silent failures*: bugs that pass tsc, eslint, and casual review. Each gets the wrong code, the right code, and why it fails silently.
4. **Check before, not after.** A pre-coding guardrail pass (scope, naming, tokens, contracts, plan gate) is cheaper than any post-hoc review.
5. **Close the loop.** Every correction becomes a lessons-learned entry the same day; repeat offenders get promoted to rules, then to linters. The pre-push checklist enforces the question: *"did any correction this session reveal a missing rule?"*
6. **Make compliance verifiable.** A "verify rules" prompt makes the agent recite which rules it loaded — a zero-pollution canary for sessions running without context.

## Provenance

The React/TS profile and most silent-failure traps come from production work on a freight-management platform (200+ companies, 10+ years of legacy code) migrating CodeIgniter/AngularJS → React + TypeScript, where I authored and operated the AI rule system. The PHP profile distills the layered-architecture standards from the same platform's modern backend services. Everything here is generalized — no proprietary code or names.

## License

MIT
