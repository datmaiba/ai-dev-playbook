# New-Project Setup — Day-1 Checklist

Wiring this playbook into a fresh (or newly joined) repo takes under an hour. In order:

## 1. Recon (15 min) — before writing any rule

- [ ] Read the repo's README, existing docs, and CI config.
- [ ] Identify: mainline branch name, how tests/lint actually run, the newest well-built module (this becomes the **reference module**).
- [ ] Note the stack(s) → pick matching `stacks/` profile(s).

## 2. Entrypoints (15 min)

- [ ] Copy `templates/CLAUDE.template.md` → `CLAUDE.md`; fill every placeholder. Prune the stack profile to rules that apply *here* — a rule about a component library the project doesn't use is noise.
- [ ] Copy `templates/cursorrules.template` → `.cursorrules` (if the team uses Cursor); keep both entrypoints consistent.
- [ ] Fill the sanctioned toolchain commands — verify each one actually runs before writing it down.

## 3. Memory (10 min)

- [ ] Copy `templates/lessons-learned.template.md` into the project's docs location.
- [ ] Seed it with any known project gotchas the team mentions in onboarding.

## 4. Guardrails & review (10 min)

- [ ] Wire `skills/pre-coding-guardrails/` into the agent setup (or paste its checklist into CLAUDE.md if skills aren't supported).
- [ ] Add `checklists/ai-code-review-checklist.md` to the PR template (prune non-applicable sections).

## 5. Verify (5 min)

- [ ] Open a fresh agent session, type **"verify rules"** — the agent should recite the entrypoint, rule files, and highest-risk rules.
- [ ] Give it a small real task and watch: does it declare scope? run guardrails? stop at the plan?

## Ongoing

- Every correction → lessons-learned entry same day (`core/02-feedback-loop.md`).
- Every push → the pre-push question: did today reveal a missing rule?
- Monthly: promote recurring entries to rules; retire rules a linter now enforces.
