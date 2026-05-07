# Handoff — bootstrap JS toolchain

> **This file is transient.** It briefs the next agent that picks up
> the JS toolchain bootstrap. Delete it as part of the same PR that
> lands the bootstrap — once the toolchain exists, this doc rots.

You're inheriting **cytometer**, a fresh agent cell at
`~/Documents/repos/cells/cytometer/`. Purpose: a web app that points
at a cell repo and runs graph algorithms + static analysis to report
on the cell's health.

## What's already been done

- Cell scaffolded from the [`stem-cell`](https://github.com/JSBaxter/stem-cell) copier template with `language=none` (JS isn't an option upstream yet — once the toolchain settles here, the operator plans to promote it into the template as a real `language: javascript` choice).
- Pushed to GitHub: https://github.com/JSBaxter/cytometer (public, `main` tracks `origin/main`).
- Queue verified: `uv sync --directory dev-tools/queue && uv run --directory dev-tools/queue pytest` → 85/85 pass.
- Commits on `main`: initial scaffold, copier source repointed to git URL with commit pin, queue lockfile pinned, copier update from template.

## Decisions already made (don't relitigate)

- **Runtime shape**: local-first, with the door open to hosted later. Operator wants to run it locally pointed at a checkout, but pick a stack that can deploy to a server when needed.
- **Framework**: **SvelteKit**. Rationale: fullstack (UI + server-side filesystem reads in one app), lightweight (less ceremony than Next.js), great local dev loop via Vite, and the adapter system means deploy-later is genuinely cheap (adapter-node for self-host, adapter-vercel/cloudflare for SaaS).
- **GitHub remote**: yes, already wired.

## Decisions still yours

- **Package manager**: pnpm recommended (fast, disk-efficient, plays well with workspaces if cytometer ever grows). Reasonable to pick npm if you want to minimize tooling surface.
- **Linter + formatter**: ESLint + Prettier (safe, max ecosystem) OR Biome (faster, single tool, simpler config, smaller ecosystem). Either is fine; record your choice + rationale.
- **Test runner**: Vitest is the obvious pick with SvelteKit/Vite.
- **Pre-commit guardrails**: husky + lint-staged is conventional; a plain `.git/hooks/pre-commit` calling `pnpm run check` is the lower-ceremony alternative.

## Plan

1. **Read the cell.** `CLAUDE.md` auto-loads — follow its reading order (MANIFESTO → CONTRIBUTING → TESTING → STATE → README). Then skim `REPRODUCTION.md` so you understand the template loop.
2. **Queue handshake** (the queue MCP server should be wired up via `.mcp.json` — call `health` first to verify):
   - `health`
   - `add_idea` (title: "bootstrap JS toolchain")
   - `scope_task`
   - `claim_task`
   - `open_session`
   - `add_note` recording the SvelteKit decision + your package-manager / linter / pre-commit choices and rationale
3. **Branch** `feat/bootstrap-js-toolchain`.
4. **Bootstrap** at the cell root (NOT inside `dev-tools/queue/`):
   - `package.json`, `tsconfig.json`
   - SvelteKit scaffold via its current init command (check Svelte docs for the up-to-date invocation)
   - Linter + formatter configs (your choice)
   - Vitest (SvelteKit's init flow includes a Vitest option — use it)
   - A `check` script that runs format-check + lint + typecheck + test
   - Pre-commit guardrail calling the `check` script
   - CI workflow at `.github/workflows/quality.yml` running install + `check` on push and PR. **The template skipped `.github/` because the spawn was `language=none`** — you have to write this from scratch.
5. **Update cell docs** to reflect JS instead of Python:
   - `CONTRIBUTING.md` — Tooling conventions section
   - `TESTING.md` — per-change-type checklists where Python tools are mentioned
   - `README.md` — directory map / notes if relevant
6. **Commit on branch, open PR.** Self-approval is fine per CONTRIBUTING.md but the PR workflow still applies. Reference the queue task ID in the PR description. Delete this `HANDOFF.md` file in the same PR.
7. **`close_session`** with outcome + a final `add_note` summarizing toolchain choices. **Do not `complete_task`** until the PR merges.

## Constraints

- Don't touch `dev-tools/queue/` — it's Python, self-contained, ships its own uv venv.
- Don't add features beyond the toolchain. No auth, no DB, no example app, no graph-algo code — those are later sessions. This task is **only** the toolchain.
- If something about the cell's purpose makes you think SvelteKit is the wrong choice (e.g. it actually needs to be a Tauri desktop app or a CLI), stop and ask the operator before committing to the stack.
- If the queue MCP isn't wired up (`health` errors, tools missing), stop — Claude Code must be running with cytometer as its working directory for `.mcp.json` to load. Ask the operator to relaunch from the cell directory.

## References

- Template: https://github.com/JSBaxter/stem-cell (HEAD `d2d0c27`, pinned in `.copier-answers.yml`)
- `CONTRIBUTING.md` / `TESTING.md` / `REPRODUCTION.md` at cell root
- Queue spec at `dev-tools/queue/SPEC.md`, workflow at `dev-tools/queue/WORKFLOW.md`
