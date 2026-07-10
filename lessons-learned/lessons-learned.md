# Lessons Learned

This file records bugs, anti-patterns, and mistakes that have been fixed in this codebase. AI agents read it before starting any coding task so the same issue is never reintroduced. It is the raw input of the rule-feedback loop (`core/02-feedback-loop.md`): recurring entries graduate into rules, then into lint checks.

**Entry format:**

- **Title**: short name for the issue (dated)
- **Context**: which file(s)/module(s) were affected
- **What went wrong**: brief description
- **The fix / what to do instead**: actionable guidance
- **Where to check before coding**: pointer to the relevant rule

---

### [example] Generic `required` validator treats `0` as empty

- **Context**: form config for a numeric-enum select field (`TYPE_A = 0 | TYPE_B = 1`).
- **What went wrong**: `required: true` uses `!value` internally — `!0 === true`, so the first enum member always failed validation even when visibly selected.
- **Fix**: for fields where `0` is valid, replace `required: true` with an explicit rule: `validate: (v) => v !== null && v !== undefined`. Keep the `required` *prop* on the JSX (it only controls the `*` indicator).
- **Where to check**: `stacks/react-typescript.md (Silent-failure traps)` — Trap 4.

---

### [example] kebab-case CSS Module keys need bracket access

- **Context**: any new `*.module.scss`.
- **What went wrong**: classes written camelCase (`.alignStart`) to allow `styles.alignStart` dot access — violating the BEM kebab-case convention; conversely, `styles.align-start` parses as subtraction.
- **Fix**: kebab-case classes + bracket access: `styles['align-start']`.
- **Where to check**: `stacks/react-typescript.md (Styling)` — BEM section.

---

<!-- New entries go above this line, newest first. Add an entry the same day the mistake is corrected. -->
