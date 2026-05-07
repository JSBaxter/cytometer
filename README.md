# cytometer

Web app that points at a cell repo and runs graph algorithms and static analysis to report on cell health.

## Start here

- **Identity & principles:** `MANIFESTO.md`
- **Rules for contributing (human or agent):** `CONTRIBUTING.md`
- **How to verify a change:** `TESTING.md`
- **Current operational truth:** `STATE.md`
- **Recurring process activities:** `CEREMONIES.md`
- **How this cell was born + how to spawn a sibling:**
  `REPRODUCTION.md`
- **Release history:** `CHANGELOG.md`

Agents: `CLAUDE.md` and `AGENTS.md` both point back at
`CONTRIBUTING.md` and `MANIFESTO.md` — those are the sources of
truth.

## Active directories

- `src/`
  SvelteKit app source — `routes/`, `lib/`, `app.html`, `app.d.ts`.
  This is where the web app lives.
- `static/`
  Static assets served as-is (favicon, robots.txt, etc.).
- `.github/workflows/`
  CI pipelines — currently `quality.yml` (format + lint + types +
  tests on every push and PR).
- `.husky/`
  Pre-commit hook config. `pnpm install` wires it up.
- `dev-tools/`
  Local-only tooling that runs on a developer's machine. Houses
  the bundled `queue/` MCP server, used by every agent working on
  this cell. Self-contained Python (uv venv) — independent of the
  cell's primary JS toolchain.

## Toolchain

- **Runtime / framework:** SvelteKit on Node ≥ 22
- **Language:** TypeScript
- **Package manager:** pnpm (pinned via `packageManager` in
  `package.json`; bootstrap with `corepack enable && pnpm install`)
- **Quality gate:** `pnpm run check` (Prettier, ESLint,
  `svelte-check`, Vitest). Wired into a Husky pre-commit hook and
  GitHub Actions.

See `CONTRIBUTING.md` for the full set of scripts and conventions.

## Reproduction

This cell was scaffolded from the
[`stem-cell`](https://github.com/JSBaxter/stem-cell).
The exact template version this cell tracks is recorded in
`.copier-answers.yml`. See `REPRODUCTION.md` for how to spawn a
sibling cell or pull template updates.

## Notes

- Local secrets and build state are gitignored.
