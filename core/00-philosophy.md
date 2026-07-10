# 00 — Philosophy

## Consistency beats speed

On any codebase that outlives a sprint, the cost center is not writing code — it's **reading, reviewing, and changing** code. AI multiplies writing speed; without rules it also multiplies reading cost, because every generation invents its own dialect. The playbook optimizes for the reader.

## Write rules like laws, not advice

- Rules live in numbered markdown files, one topic each, with an index.
- RFC 2119 keywords: **MUST / MUST NOT** violations block the merge; **SHOULD** deviations need a stated reason; **MAY** is optional.
- Every rule that can be checked by a machine eventually should be — prose is the incubator, the linter is the destination.

Why the strong language matters for AI specifically: agents deprioritize soft guidance as context grows. An instruction phrased as a preference will be traded away against the user's immediate request; an instruction phrased as a blocking requirement will not.

## Rules must explain WHY

A rule that only states *what* invites "fixes" from people (and agents) who don't see the reason. Each rule carries its failure story: what breaks without it, and — for silent failures — why nothing catches it. The why is also what makes rules teachable: new team members read the rules and inherit years of debugging in an afternoon.

## The agent is a brilliant amnesiac

Treat every session as a skilled developer with **zero memory** of the project. Everything it must know either lives in a file it auto-loads (`CLAUDE.md`, `.cursorrules`) or in a file those point to. If knowledge lives only in someone's head or a chat history, it does not exist.

Three consequences:

1. **Entrypoints are onboarding documents** — 30-second orientation, pointers to the law, the most-violated rules inlined.
2. **A lessons-learned log is the agent's long-term memory** — every scar recorded, read before every task.
3. **Compliance must be verifiable** — "verify rules" makes the agent recite its loaded rules; a session that can't recite them is running blind and must not write code.

## Humans stay in the loop at three points

1. **Before code**: plan approval (plan-gated execution — `core/01`).
2. **During code**: scope questions when the agent wants to cross a boundary.
3. **After code**: the 2-minute review checklist.

Everything else is the agent's job — including running the checks.
