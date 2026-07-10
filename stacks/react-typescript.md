# Stack Profile — React + TypeScript

Plug-in rule profile for React 18 + TypeScript frontends. Assumes: React Query (server state), a small client store (UI state), CSS Modules or Tailwind with design tokens, TanStack Table or similar.

## Architecture (MUST)

```
component → hook → api
```

Components render and delegate to hooks; hooks own logic and delegate I/O to `api/` functions; `api/` functions use the shared HTTP client. No `fetch`/`axios` in components or hooks. Server data lives in React Query — never copied into a client store (two sources of truth always diverge).

**Module structure**: each page/module owns its `api/`, `hooks/`, `stores/`, `components/`, `types.ts`. Cross-module reuse goes through `shared/` — sibling imports are forbidden.

**Component folder standard**: one PascalCase folder per component — `ComponentName.tsx` + styles + `index.ts` barrel; **named exports only** (default exports let every importer invent a different name, killing grep and safe renames). Components SHOULD stay under 200 lines; non-trivial logic MUST live in hooks.

**Reuse before writing**: grep `shared/components/` before writing any styled element. Duplicating an existing shared component is a blocking defect. No raw `<table>` HTML — shared table components own search, selection, sorting, pagination.

## Naming

| What | Rule | Example |
|------|------|---------|
| Component folder/file | PascalCase, matches export | `DriverForm/DriverForm.tsx` |
| Hook | `use` + purpose | `useDriverFormMutation` |
| API function | verb + entity | `fetchDrivers`, `createDriver` |
| Query key | `[module, entity, variant, …params]` | `['drivers', 'detail', id]` |
| Boolean | `is` / `has` / `can` | `isLoading`, `canEdit` |
| CSS class | BEM kebab-case | `.driver-form__field--error` |
| Type | PascalCase, no `I` prefix | `DriverFilters` |

## Styling: tokens only (MUST)

Colors, spacing, font sizes, radii, shadows, z-indexes, transitions — all from design tokens (`var(--…)`) or project mixins. Zero raw hex/px. Lookup order: exact token → closest *semantic* token (pick by role, not by number) → **stop and ask**; agents never invent tokens. Inline `style={{…}}` forbidden outside documented runtime exceptions. This is the #1 AI styling mistake because hardcoded values look correct and only explode at theme time.

## Silent-failure traps (each passed tsc + eslint + review before being caught)

1. **0-based paging**: table engines call fetchers with 0-based `page`. Slicing is `start = page * pageSize` — the "intuitive" `(page - 1)` version serves page 1 twice and only fails when a user clicks page 2. Return `{ data, total }`. Smoke test: pages 1→2→3, first row id must change.
2. **Custom `cell` vs `meta.type`**: a column with a custom `cell` renderer MUST use `meta.type: 'custom'` (or omit) — any built-in type silently discards the renderer. Both fields are individually valid, so nothing warns.
3. **Two features, one interaction**: never enable both a high-level feature and its low-level building block (e.g. `rowReorder` and raw `rowDrag`) — one silently wins, the other never fires.
4. **`required` and falsy valids**: generic `!value` validators treat `0` as empty — numeric enums starting at 0 always fail. Use explicit `v !== null && v !== undefined`.
5. **Wizard modal contract**: every step — including the last — finishes with `onNext(patch)`; calling `close(result)` on the final step skips the last merge and loses that step's data.

## Testing & tooling

- New/extracted components: evaluate whether a co-located test (Vitest + Testing Library) and Storybook story are required; shared components always get both.
- All Node commands via the sanctioned toolchain path (`core/03-environment.md`).
