# Hamy

A personal assistant I open every morning. It tells me what I missed, what's
going on in the world, what's on my plate today, and teaches me one thing
worth knowing in my field.

Everything it learns gets written back into an Obsidian vault as linked
markdown notes — so over time it builds a visual map of my knowledge as a
side effect of just being used.

> This is also a deliberate exercise in building software the way a real team
> would: one feature per branch, every change through a pull request, and a
> CI/CD pipeline with actual security gates. The pipeline is as much the point
> as the product.

## Stack

| Layer | Choice |
|---|---|
| API | Python · FastAPI |
| Web | React · TypeScript · Vite |
| Knowledge store | Obsidian vault (markdown), synced via a private Git repo |
| Hosting | GCP Cloud Run |
| CI/CD | GitHub Actions |

## Architecture

```
Obsidian vault (my Mac)
        │  auto commit + push
        ▼
  private vault repo
        │  pull
        ▼
  Hamy API  (FastAPI)
   /brief   weather · news · tasks
   /graph   nodes + edges from the vault
   /lesson  daily "teach me something"
        │  JSON over HTTPS
        ▼
  Hamy Web  (React)
```

The vault is the source of truth; the database is only ever a cache. If Hamy
disappeared tomorrow, every note would still be sitting on disk in plain text.

## Repo layout

```
apps/
  api/    FastAPI service
  web/    React frontend
docs/     design notes and decisions
.github/
  workflows/    CI/CD
```

A monorepo, so a change to an API response and the frontend that reads it
land in the same pull request and can't drift apart. CI uses path filters —
touching `apps/web` doesn't rebuild the API.

## Status

Phase 0 — foundations. No features yet; building the pipeline first so that
shipping one is boring.

- [x] Repo + skeleton
- [ ] Branch protection
- [ ] Commit conventions, PR template
- [ ] Python toolchain (uv, ruff, mypy, pytest)
- [ ] Web toolchain (pnpm, ESLint, Prettier, Vitest)
- [ ] pre-commit hooks
- [ ] CI: lint, typecheck, test, build
- [ ] Security gates: gitleaks, Dependabot, CodeQL, keyless GCP auth
- [ ] CD to Cloud Run (dev → staging, main → production)

Then: Phase 1 morning brief · Phase 2 the knowledge graph · Phase 3 tasks and
calendar · Phase 4 the LLM layer.

## Branches

- `main` — production
- `dev` — staging
- feature branches off `dev`

## Getting started

Not yet — there's nothing to run. This section arrives with Phase 1.

## License

None yet. The repo is private while Phase 0 is in progress; a license lands in
the PR that makes it public.
