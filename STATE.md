# State — cytometer

This file is **operational truth**. What this cell currently runs,
exposes, depends on, and stores. Keep it accurate; update it in the
same PR as any change that affects what's live.

If a section is empty, leave the heading and write "Nothing yet."
The structure stays even when the content doesn't.

---

## What this cell does today

Web app that points at a cell repo and runs graph algorithms and static analysis to report on cell health.

The app itself is not yet implemented — the cell currently ships
only a JS toolchain and a SvelteKit dev shell (default scaffold,
single landing page). No graph algorithms, no static analysis, no
deploy target picked yet.

---

## What's running

- A SvelteKit dev server, locally only, via `pnpm run dev`. Not
  exposed to a network or a deployment target.
- The `quality` GitHub Actions workflow on
  `https://github.com/JSBaxter/cytometer`, running `pnpm run check`
  on every push to `main` and on every PR.
- A Husky pre-commit hook (`.husky/pre-commit`) running
  `pnpm run check` locally on every commit attempt.

---

## Dependencies

### Build / runtime

- **Node** `≥ 22` — required by `engines.node` in `package.json`.
- **pnpm** — version pinned via the `packageManager` field;
  installed automatically by Corepack on `pnpm install`.
- **SvelteKit + Vite + Svelte 5** — declared in `package.json`,
  resolved via `pnpm-lock.yaml`.
- **Adapter:** `@sveltejs/adapter-auto` (default scaffold; will be
  swapped for a concrete adapter when a deploy target is picked).

### External

Nothing yet — the app makes no outbound network calls.

---

## Where secrets live

Secrets are **never** committed. Possible homes:

- A password manager (1Password, Bitwarden, etc.) — operator
  workstation only
- An environment variable on the deployment target
- A secret store the cell explicitly authenticates against

When the cell starts using a secret, list it here with its source
(not its value):

```
| Secret              | Source                                    |
|---------------------|-------------------------------------------|
| GITHUB_TOKEN        | Operator's gh CLI auth                    |
| ...                 | ...                                       |
```

---

## What's NOT under this cell's control

- Anything outside this repo
- Anything the operator runs on their own machine and hasn't
  committed here

---

## Drift log

When `STATE.md` doesn't match reality and the discrepancy can't be
fixed inline, log it here with a date and a tracked task ID:

```
- 2026-MM-DD — <description> — task_xxxxxx
```

(Empty until something drifts.)
