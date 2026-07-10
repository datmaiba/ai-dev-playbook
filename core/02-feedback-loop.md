# 02 — The Feedback Loop

The most important file in the playbook. Static rule sets rot: codebases change, libraries change, models change. The only thing that keeps rules alive is a mechanism that feeds corrections back into them.

## The loop

```
agent makes a mistake → human corrects it → SAME DAY:
  1. add a lessons-learned entry (what broke, the fix, where to check)
  2. 2nd+ occurrence      → promote to a rule file with MUST language
  3. rule already existed → the rule was unclear; rewrite the rule
  4. still recurring      → enforce with a linter; shrink prose to a pointer
```

Branch 3 is the subtle one. When an agent violates an *existing* rule, the reflex is to blame the agent — the correct diagnosis is usually that the rule is ambiguous, buried, or missing its "why". Rewriting a rule once is cheaper than re-correcting the same mistake forever.

## Making the loop mandatory

Aspirational loops don't run. Two enforcement points:

- **Pre-push checklist** includes: *"Did any agent correction this session reveal a missing or wrong rule? If yes — update the rule file before pushing."* Every push becomes a checkpoint asking whether the rule set just got smarter.
- **Review checklist** ends with: every correction has a lessons-learned entry; recurring mistakes have been promoted.

## The lessons-learned log

Format per entry: dated title · context (files/modules) · what went wrong · the fix / what to do instead · pointer to the rule. Newest first. Written the same day — tomorrow the details are gone, next week the whole bug is.

The log is the agent's long-term memory: guardrail #1 makes every session read it before coding and surface relevant hits explicitly, so inherited scars actually prevent repeat injuries.

## A rule's life cycle

```
incident → log entry → prose rule (MUST) → lint rule → deleted prose (one-line pointer)
```

The destination of a good rule is to disappear from the documents because a machine enforces it. Prose is for what machines can't yet check — and for the WHY, which stays even after the linter takes over.
