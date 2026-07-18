# Status Report — 2026-05-02 22:57

## A) Fully Done

| #   | Item                                      | Details                                                                                                                                                                                                                                                          |
| --- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Achievements "Unexpected error" fixed** | Root cause: GitHub deprecated Projects (classic) API. Switched `lowlighter/metrics@latest` → `dkhokhlov/metrics@master` (PR #1769 with Projects V2 migration). User added `read:project` scope to `METRICS_TOKEN`. Verified: zero errors in last successful run. |
| 2   | **Trophies rendering fixed**              | `github-profile-trophy-kannan.vercel.app` was dead (404). Now generates SVG in-workflow via `Erik-Donath/github-profile-trophy@feature/generate-svg` with `METRICS_TOKEN` — includes private repo data. SVG committed to repo.                                   |
| 3   | **Featured projects updated**             | Changed from `web-client-errors-mcp, clean-wizard, art-dupl, template-sqlc` → `dynamic-markdown-site, go-filewatcher, art-dupl, emeet-pixyd, go-branded-id`. All verified to exist.                                                                              |
| 4   | **Workflow end-to-end verified**          | Last 2 runs succeeded. All 3 SVGs generated: `metrics.svg`, `metrics.repositories.svg`, `trophies.svg`. All committed to repo. README references them correctly.                                                                                                 |

## B) Partially Done

| #   | Item                             | Status      | What's Left                                                                                                                                                                                                             |
| --- | -------------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Security: Action SHA pinning** | NOT STARTED | `dkhokhlov/metrics@master` and `Erik-Donath/github-profile-trophy@feature/generate-svg` are branch refs — if either account is compromised, arbitrary code runs with full `repo`-scoped token. Must pin to commit SHAs. |
| 2   | **Stale third-party fork**       | TRACKED     | `dkhokhlov/metrics@master` is a temporary fork. Should revert to `lowlighter/metrics@latest` once PR lowlighter/metrics#1769 merges. Needs periodic check.                                                              |

## C) Not Started

| #   | Item                                    | Impact  | Notes                                                                                                                                                |
| --- | --------------------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Light/dark theme for metrics SVG**    | Medium  | Both `<source>` tags in README point to the same dark `metrics.svg`. Could generate a light variant too.                                             |
| 2   | **2025 GitHub Wrapped image**           | Low     | Section shows 2022–2024 but not 2025.                                                                                                                |
| 3   | **`typespec-asyncapi` not featured**    | Medium  | 12 stars — most-starred repo by far. Not in featured list.                                                                                           |
| 4   | **README About Me code block accuracy** | Low     | Lists languages/interests that may be outdated.                                                                                                      |
| 5   | **`committer_message` customization**   | Low     | Metrics action uses default commit message; could use `[Skip GitHub Action]` to prevent recursive triggers.                                          |
| 6   | **AGENTS.md in this repo is outdated**  | Medium  | Project-level AGENTS.md is v4.1, global is v5.0. Content diverges (mentions `justfile`, wrong memory path `~/.picoclaw/workspace/memory/MEMORY.md`). |
| 7   | **`docs/` directory**                   | New     | Created for this report. Could hold architecture decisions, status reports, etc.                                                                     |
| 8   | **`.gitignore` is Go-specific**         | Low     | Contains Go patterns (`*.test`, `vendor/`, `go.work`) but this is a docs/profile repo. Not harmful but misleading.                                   |
| 9   | **SETUP.md review**                     | Unknown | Haven't read it yet — may be outdated.                                                                                                               |

## D) Totally Fucked Up

| # | Item | Severity | Details |
| --- | ------------------------------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 | **Trophy file name mismatch** | Medium | Trophy action writes `./trophy.svg` (ignores `file` input). Commit step copies it with `cp trophy.svg trophies.svg 2>/dev/null                                                                                                                                                                                                            |     | true`. This silently swallows errors. If the action ever fixes this or changes behavior, the copy breaks silently. Should investigate if `file` input actually works or remove it. |
| 2 | **Workflow triggers on every push to main** | Medium | `push: { branches: ["main", "master"] }` means the metrics workflow triggers itself (it pushes SVGs to main). The `[Skip GitHub Action]` in the commit message is NOT checked by the workflow — it's a convention-only guard. The `concurrency` group with `cancel-in-progress: true` mitigates infinite loops but wastes runner minutes. |

## E) What We Should Improve

### Security

- **Pin all actions to commit SHAs** (not branch tags) — supply chain attack prevention
- **Reduce METRICS_TOKEN scope** — currently `repo` (full read/write all repos). Fine-grained PAT scoped to only this repo would be safer
- **Add `if` guard** to skip workflow when commit message contains `[Skip GitHub Action]`

### Reliability

- **Remove self-triggering** — either remove `push` trigger or add `[Skip GitHub Action]` check
- **Fix trophy file path** — verify if `file` input works; if not, file an issue upstream
- **Add error handling** in commit step — don't `|| true` the copy

### Quality

- **Sync project AGENTS.md** with global v5.0 or remove it (project-level overrides global)
- **Clean .gitignore** to match actual repo content (no Go code here)
- **Review SETUP.md** for accuracy

### Presentation

- **Add `typespec-asyncapi`** to featured projects (12 stars, most popular)
- **Generate light theme SVGs** for the `<picture>` elements
- **Update "About Me"** Kotlin block if skills/focus have changed

## F) Top 25 Things to Do Next

Sorted by: **(Impact × Effort) — highest value first**

| Priority | Item | Impact | Effort | Type |
| -------- | -------------------------------------------------------------------------- | ------- | ------ | ------------- | --- | ----------- |
| 1 | Pin actions to commit SHAs | HIGH | LOW | Security |
| 2 | Add `[Skip GitHub Action]` guard to workflow trigger | HIGH | LOW | Reliability |
| 3 | Remove `push` trigger (use `schedule` + `workflow_dispatch` only) | HIGH | LOW | Reliability |
| 4 | Update project AGENTS.md to v5.0 or remove it | MEDIUM | LOW | Maintenance |
| 5 | Clean `.gitignore` for this repo type | LOW | LOW | Hygiene |
| 6 | Review SETUP.md for accuracy | UNKNOWN | LOW | Maintenance |
| 7 | Fix trophy file copy error handling (remove `                              |         | true`) | MEDIUM | LOW | Reliability |
| 8 | Add `typespec-asyncapi` to featured projects | MEDIUM | LOW | Presentation |
| 9 | Check if lowlighter/metrics#1769 has merged | MEDIUM | LOW | Maintenance |
| 10 | Generate light-theme SVGs for `<picture>` elements | MEDIUM | MEDIUM | Presentation |
| 11 | Reduce METRICS_TOKEN to fine-grained PAT | HIGH | MEDIUM | Security |
| 12 | Update About Me Kotlin block | LOW | LOW | Presentation |
| 13 | Add 2025 GitHub Wrapped image | LOW | LOW | Presentation |
| 14 | Add `committer_message` with `[Skip GitHub Action]` to metrics action | LOW | LOW | Reliability |
| 15 | Verify trophy action `file` input behavior — file upstream issue if broken | MEDIUM | MEDIUM | Reliability |
| 16 | Add `retries: 3` to metrics action for transient API failures | LOW | LOW | Reliability |
| 17 | Add workflow step to verify SVGs are valid before committing | MEDIUM | MEDIUM | Reliability |
| 18 | Consider adding WakaBox/stats for coding time | LOW | MEDIUM | Presentation |
| 19 | Consider adding GitHub Skyline 3D contribution graph | LOW | MEDIUM | Presentation |
| 20 | Add dependabot for GitHub Actions version updates | MEDIUM | LOW | Security |
| 21 | Add `workflow_dispatch` inputs for manual force-refresh | LOW | LOW | DX |
| 22 | Test with `pull_request` trigger to validate SVG generation before merge | MEDIUM | MEDIUM | CI/CD |
| 23 | Add badge showing last successful workflow run | LOW | LOW | Presentation |
| 24 | Consider self-hosting github-profile-trophy on Vercel for faster updates | LOW | HIGH | Architecture |
| 25 | Document the full architecture in `docs/architecture.md` | LOW | MEDIUM | Documentation |

## G) My Top #1 Question I Cannot Figure Out Myself

**Should the `dkhokhlov/metrics` fork be a permanent dependency, or is there a timeline to revert to `lowlighter/metrics`?**

The fork PR (lowlighter/metrics#1769) has been open since Nov 2025, last updated Jan 2026, and is `mergeable: true` but unmerged. This is your call:

- If it merges soon → we revert and pin to upstream SHA
- If it won't merge → we should pin the fork SHA and consider it permanent
- Either way: pin the SHA now for security

---

## Session Timeline

| Time (UTC) | What Happened                                                                                             |
| ---------- | --------------------------------------------------------------------------------------------------------- |
| ~20:00     | Investigated achievements "Unexpected error" — found Projects (classic) deprecation                       |
| ~20:09     | Switched to `dkhokhlov/metrics@master`, pushed, first run still errored (missing `read:project` scope)    |
| ~20:30     | User added `read:project` scope, re-ran workflow — achievements working, no errors                        |
| ~20:35     | Found trophies using dead `kannan` fork (404), switched to official `vercel.app` URL                      |
| ~20:45     | Updated featured projects per user request                                                                |
| ~20:50     | Decided to self-host trophy generation in-workflow for private repo access                                |
| ~21:10     | Trophy action ran but SVG wasn't committed (action doesn't auto-commit)                                   |
| ~21:20     | Added checkout + commit step, restructured workflow                                                       |
| ~21:35     | Commit step failed — `trophies.svg` not found (action writes `trophy.svg`, wrong input names)             |
| ~21:45     | Fixed input names (`file` not `output_path`, `no-background` not `no-bg`, etc.) and added `cp` workaround |
| ~21:50     | Full workflow succeeded — all 3 SVGs generated and committed                                              |
| ~22:57     | Status report written                                                                                     |
