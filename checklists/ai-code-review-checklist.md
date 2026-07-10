# AI-Generated Code Review Checklist

A 2-minute yes/no pass for reviewing code written by AI agents. Any "no" blocks the merge.

## Scope
- [ ] Only the declared files/modules were touched.
- [ ] No unrelated refactors or "improvements" snuck in.

## Conventions
- [ ] Naming follows the matching `stacks/` profile (folders, hooks, API functions, booleans, CSS classes).
- [ ] Named exports only; component folder standard respected.
- [ ] Matches the patterns of recently written code — didn't invent a new structure.
- [ ] No duplicate of an existing shared component; no raw `<table>` HTML.

## Styling
- [ ] Every color/spacing/font/radius/shadow value is a token or mixin — zero raw values.
- [ ] No inline `style={{…}}` outside documented exceptions.

## Data & types
- [ ] API calls go through the shared client, via the component → hook → api flow.
- [ ] No `any`; response types declared.
- [ ] Paged fetchers: 0-based, `{ data, total }`, `start = page * pageSize`; pages 1→2→3 smoke-tested.
- [ ] Custom `cell` renderers use `meta.type: 'custom'` (or omit it).

## Hygiene
- [ ] No ticket IDs, design-node refs, old/new markers, or commented-out code.
- [ ] No copy-pasted logic that should be an extraction.
- [ ] Required tests/stories exist and pass — run via the sanctioned toolchain (core/03).

## Meta (the feedback loop)
- [ ] Every correction made during this review has a lessons-learned entry.
- [ ] Recurring mistakes have been promoted to a rule (or a linter).
