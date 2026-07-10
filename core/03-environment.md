# 03 — Environment & Toolchain

## One sanctioned path (MUST)

All toolchain commands — type-check, tests, lint, build, scaffolding — run through the project's sanctioned execution path. In our case that is the dev container:

```bash
docker exec <frontend-container> sh -c "cd /app && npx tsc --noEmit"
docker exec <frontend-container> sh -c "cd /app && npm test -- --run"
docker exec <frontend-container> sh -c "cd /app && npx eslint src/"
```

- MUST NOT run `npm` / `npx` / `node` on the host for this repo.
- MUST NOT hunt the host for alternative binaries (nvm, Homebrew…) as a workaround.
- If the container is not running: **say so and ask** — do not fall back silently.

## Why agents need this rule

An AI agent blocked by a missing tool will creatively route around it — the wrong Node version, a global binary, a different package manager. Every workaround produces results that differ from CI and wastes a review cycle. "One sanctioned path + stop when it's unavailable" converts silent drift into a visible, fixable condition.

## Version pinning

Tool versions live in the container image / lockfiles, not in developer memory. The agent never "upgrades while it's there" — dependency changes are their own task with their own review.
