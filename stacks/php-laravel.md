# Stack Profile — PHP / Laravel (+ legacy CodeIgniter notes)

Plug-in rule profile for PHP 8.2+ / Laravel backends, with a section for working inside legacy PHP monoliths.

## Layered architecture (MUST, one-way)

```
Controllers → Services → Repositories → Models
```

- **Controllers**: one API per controller, a single public `__invoke` method. A controller that does two things is two controllers.
- **Services** split CQRS-lite:
  - **Command services** — one write operation each, single `execute()` method, no interfaces.
  - **Query services** — read-only; a query service that mutates is a blocking defect.
- **Repositories**: return plain objects; no business logic; **no Eloquent leakage** upward (callers must never receive a live model they can lazy-load or `save()`).
- Forbidden: service interfaces "for testability" (concrete classes mock fine), `BaseService` inheritance trees, service-locator calls (`app()`) inside services — dependencies are constructor-injected.

Why this profile is AI-critical: Laravel permits everything everywhere (facades, `Model::query()` in controllers, logic in models). An agent trained on tutorial code will happily produce it. The one-way arrow gives the agent — and the reviewer — a single question per file: *does anything here reach backward?*

## Routes & permissions

- One `routes/<entity>.php` per entity; group endpoints under read/write scopes.
- Permissions via `Permission::` constants — raw permission strings are FORBIDDEN (typos become runtime security holes; constants fail at parse time and grep cleanly).

## Database

- Schema source of truth is the shared schema library (one `.sql` file per table) — table/column names in code MUST match it. Naming disputes end at the file.
- Schema changes follow the documented DB-change process; they are never bundled into feature PRs.
- Read/write replica separation where the platform provides it — writes to the writer, tolerate replica lag on reads.

## Testing

- PHPUnit via the sanctioned toolchain: `docker exec <container> ./vendor/bin/phpunit` — never host PHP (`core/03-environment.md`).
- New code ships with tests; test names describe behavior, not method names.

## Working in a legacy monolith (CodeIgniter-era rules)

1. **Reference module first**: before building anything, read the designated reference module and copy its patterns. In a 10+ year codebase there are five generations of style — the reference module defines which one is current. Never pattern-match against a random old module.
2. **Campsite rule**: leave every file slightly cleaner than found — but *scoped*: opportunistic cleanup stays within the task's files; anything bigger is proposed separately.
3. **Preserve behavior exactly** when porting: log behavioral differences you notice instead of silently "fixing" them — that "bug" may be load-bearing.
4. **Don't carry anti-patterns forward**: inline SQL, global state, echo-in-model — legacy code is context, not precedent.
5. **Docblocks explain why**, not what. No ticket references, no commented-out corpses — git remembers.

## Naming

PSR-12 baseline. Command services: `Create<Entity>Service`, `Update<Entity>Service`. Query services: `Get<Entity>Service`, `List<Entities>Service`. Repositories: `<Entity>Repository`. Booleans `is/has/can`. Database: snake_case matching the schema library.
