# CLAUDE.md — <PROJECT NAME>

<!-- TEMPLATE: copy to your repo root as CLAUDE.md and fill every <PLACEHOLDER>.
     Keep it under ~150 lines: point at rules, inline only the most-violated ones. -->

This file provides guidance to Claude Code when working with code in this repository.

## Mandatory rules

**Before making any changes**, read the rules under `<RULES_DIR e.g. docs/rules/>` — they contain blocking requirements. Rules use RFC 2119 keywords; MUST / MUST NOT violations are blocking defects. On conflict, follow the precedence chain in `<PRECEDENCE_DOC>`.

| File | Topic |
|------|-------|
| `<RULES_DIR>/…` | <architecture / styling / data contracts / environment / workflow> |

**Before writing code**: run the pre-coding guardrails (`<GUARDRAILS_PATH>`) and read `<LESSONS_LEARNED_PATH>`.

## What this repo is

<ONE PARAGRAPH: what the system does, who uses it, where it runs.>

## Tech stack

<BULLET LIST: language + version, framework, key libraries, build tool, test framework.>

## Architecture (most-violated rules — keep in context)

<INLINE 5–10 MUST rules the agent breaks most often. Start from the matching stacks/ profile
 in the playbook and prune to what applies. Examples:>

- **Flow (MUST)**: <component → hook → api  |  Controllers → Services → Repositories → Models>
- **<naming rule>**
- **<reuse-before-writing rule>**
- **<styling tokens rule>**

## Silent-failure traps

<COPY the traps from the relevant stacks/ profile that exist in this codebase; add project-specific
 ones as they are discovered. Each: wrong code, right code, why nothing catches it.>

## Environment (MUST)

All build/test/lint commands via: `<SANCTIONED COMMAND, e.g. docker exec <container> …>`.
MUST NOT run tools on the host; if the sanctioned path is unavailable, say so and stop.

```bash
<COMMAND: type-check>
<COMMAND: run tests>
<COMMAND: lint>
```

## Scope control

Declare files you will touch before editing. Ask before modifying `<SHARED_PATHS>`, configs, or other modules. No drive-by refactors.

## When asked for a plan

Present the plan only — scope, files, steps, risks — and STOP until explicitly approved.

## Rule verification

When the user says **"verify rules"**, respond with a short checklist of loaded rule files and recite the highest-risk rules. Do not generate code.
