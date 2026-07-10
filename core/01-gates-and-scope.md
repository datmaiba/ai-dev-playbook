# 01 — Gates, Scope & Plan-Gated Execution

## Rule precedence

Rules exist at up to three levels — organization > project > repository. Higher levels cannot be bypassed or weakened by lower ones. On conflict: follow the higher rule **and flag the conflict** so the stale rule gets fixed. An agent silently picking a side preserves the conflict forever.

## Non-bypassable gates

Some checks are hard gates: if they fail — or anyone asks to skip them ("just this once", "we're in a hurry") — the agent refuses development work until they pass. Typical gates: standards repo present and freshly pulled; workspace layout correct; pre-coding guardrails run.

Gates only work with **zero exceptions**. One tolerated bypass teaches everyone that every rule is negotiable.

## Scope control

1. **Primary scope** = folders the task names explicitly.
2. **Implicit scope** = direct dependencies — free to *read*, editing crosses the boundary.
3. **Out of scope** = shared code, app bootstrap, other modules, root configs.

Before touching an out-of-scope file, the agent pauses and asks, stating the path and a one-sentence reason. No silent edits to shared code — even a "trivial" barrel export. No drive-by refactors: unrelated improvements are proposed separately, never bundled.

Why this is an AI rule: a shared file is a change to N modules at once, and the person who assigned the task was probably thinking about one. The scope question forces the blast radius to be seen before it happens.

## Plan-gated execution

When asked for a plan (any language, any phrasing), the agent produces the plan — scope, files to touch, steps, risks, open questions — and **stops**. Read-only investigation is fine; file mutation is not. Implementation starts only on explicit approval; ambiguous replies get a clarifying question. Plan + execution never ship in one turn.

Two supporting habits:

- **Plan self-review**: every plan is checked against the full guardrail list *before* being presented — never present a plan with known problems for the user to catch.
- **Re-present on change**: if discussion materially changes the plan, the updated plan is shown again before implementation.
