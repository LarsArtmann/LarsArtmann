# Status Report — 2026-05-02 22:57

> Archived 2026-09-30 (docs-health pass): every item below carries an inline verdict — strikethrough = resolved, `→ TODO_LIST`/`→ ROADMAP` = forwarded open work. Open items live in `TODO_LIST.md` and `ROADMAP.md`.

## A) Fully Done

| #  | Item | Details                                   |
| -- | ---- | ----------------------------------------- |
| ~~ | 1    | **Achievements "Unexpected error" fixed** |
| ~~ | 2    | **Trophies rendering fixed**              |
| ~~ | 3    | **Featured projects updated**             |
| ~~ | 4    | **Workflow end-to-end verified**          |

## B) Partially Done

| #  | Item | Status                           | What's Left |
| -- | ---- | -------------------------------- | ----------- |
| ~~ | 1    | **Security: Action SHA pinning** | NOT STARTED |
| ~~ | 2    | **Stale third-party fork**       | TRACKED     |

## C) Not Started

| #  | Item | Impact                                  | Notes   |
| -- | ---- | --------------------------------------- | ------- |
| ~~ | 1    | **Light/dark theme for metrics SVG**    | Medium  |
| ~~ | 2    | **2025 GitHub Wrapped image**           | Low     |
| ~~ | 3    | **`typespec-asyncapi` not featured**    | Medium  |
| ~~ | 4    | **README About Me code block accuracy** | Low     |
| ~~ | 5    | **`committer_message` customization**   | Low     |
| ~~ | 6    | **AGENTS.md in this repo is outdated**  | Medium  |
| ~~ | 7    | **`docs/` directory**                   | New     |
| ~~ | 8    | **`.gitignore` is Go-specific**         | Low     |
| ~~ | 9    | **SETUP.md review**                     | Unknown |

## D) Totally Fucked Up

| # | Item | Severity | Details |
| --- | ------------------------------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
~~| 1 | **Trophy file name mismatch** | Medium | Trophy action writes `./trophy.svg` (ignores `file` input). Commit step copies it with `cp trophy.svg trophies.svg 2>/dev/null                                                                                                                                                                                                            |     | true`. This silently swallows errors. If the action ever fixes this or changes behavior, the copy breaks silently. Should investigate if `file` input actually works or remove it. |~~ → TODO_LIST T1 — still open (workaround present at metrics.yml:81)
~~| 2 | **Workflow triggers on every push to main** | Medium | `push: { branches: ["main", "master"] }` means the metrics workflow triggers itself (it pushes SVGs to main). The `[Skip GitHub Action]` in the commit message is NOT checked by the workflow — it's a convention-only guard. The `concurrency` group with `cancel-in-progress: true` mitigates infinite loops but wastes runner minutes. |~~ done at 9ea8677 — workflow_dispatch-only now (metrics.yml:5-6)

## E) What We Should Improve

_Statuses are tracked per-item in section F below; every item there is now annotated (2026-09-30)._

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
~~| 1 | Pin actions to commit SHAs | HIGH | LOW | Security |~~ done at 9ea8677
~~| 2 | Add `[Skip GitHub Action]` guard to workflow trigger | HIGH | LOW | Reliability |~~ done — obsolete: auto-triggers removed entirely (metrics.yml:3-6)
~~| 3 | Remove `push` trigger (use `schedule` + `workflow_dispatch` only) | HIGH | LOW | Reliability |~~ done at 9ea8677
~~| 4 | Update project AGENTS.md to v5.0 or remove it | MEDIUM | LOW | Maintenance |~~ done 2026-09-30 — rewritten as repo-specific context
~~| 5 | Clean `.gitignore` for this repo type | LOW | LOW | Hygiene |~~ → TODO_LIST T4 — still open
~~| 6 | Review SETUP.md for accuracy | UNKNOWN | LOW | Maintenance |~~ done 2026-09-30 — reviewed and corrected
~~| 7 | Fix trophy file copy error handling (remove `|         | true`) | MEDIUM | LOW | Reliability |~~ → TODO_LIST T1 — still open
~~| 8 | Add `typespec-asyncapi` to featured projects | MEDIUM | LOW | Presentation |~~ done 2026-09-30 — now featured in README
~~| 9 | Check if lowlighter/metrics#1769 has merged | MEDIUM | LOW | Maintenance |~~ done 2026-09-30 — verified still unmerged via API; fork retained, TODO_LIST T3
~~| 10 | Generate light-theme SVGs for `<picture>` elements | MEDIUM | MEDIUM | Presentation |~~ Won't implement — moot while SVGs are not displayed
~~| 11 | Reduce METRICS_TOKEN to fine-grained PAT | HIGH | MEDIUM | Security |~~ → TODO_LIST T2 — still open (needs GitHub settings action by Lars)
~~| 12 | Update About Me Kotlin block | LOW | LOW | Presentation |~~ Won't implement — About Me block removed in d68c405
~~| 13 | Add 2025 GitHub Wrapped image | LOW | LOW | Presentation |~~ Won't implement — Wrapped section removed in d68c405
~~| 14 | Add `committer_message` with `[Skip GitHub Action]` to metrics action | LOW | LOW | Reliability |~~ done — [Skip GitHub Action] present in commit steps (metrics.yml:83)
~~| 15 | Verify trophy action `file` input behavior — file upstream issue if broken | MEDIUM | MEDIUM | Reliability |~~ → TODO_LIST T1 — still open
~~| 16 | Add `retries: 3` to metrics action for transient API failures | LOW | LOW | Reliability |~~ → ROADMAP — workflow polish when metrics return
~~| 17 | Add workflow step to verify SVGs are valid before committing | MEDIUM | MEDIUM | Reliability |~~ → ROADMAP — workflow polish when metrics return
~~| 18 | Consider adding WakaBox/stats for coding time | LOW | MEDIUM | Presentation |~~ → ROADMAP — WakaBox idea
~~| 19 | Consider adding GitHub Skyline 3D contribution graph | LOW | MEDIUM | Presentation |~~ → ROADMAP — Skyline idea
~~| 20 | Add dependabot for GitHub Actions version updates | MEDIUM | LOW | Security |~~ done at 9ea8677 — dependabot.yml, weekly, github-actions
~~| 21 | Add `workflow_dispatch` inputs for manual force-refresh | LOW | LOW | DX |~~ done — workflow_dispatch present (metrics.yml:5-6)
~~| 22 | Test with `pull_request` trigger to validate SVG generation before merge | MEDIUM | MEDIUM | CI/CD |~~ Won't implement — no PR flow for generated artifacts; manual dispatch suffices
~~| 23 | Add badge showing last successful workflow run | LOW | LOW | Presentation |~~ Won't implement — SVGs not displayed
~~| 24 | Consider self-hosting github-profile-trophy on Vercel for faster updates | LOW | HIGH | Architecture |~~ → ROADMAP — self-host trophy idea
~~| 25 | Document the full architecture in `docs/architecture.md` | LOW | MEDIUM | Documentation |~~ NOT-DO — profile repo needs no architecture doc; context lives in AGENTS.md

## G) My Top #1 Question I Cannot Figure Out Myself

**Should the `dkhokhlov/metrics` fork be a permanent dependency, or is there a timeline to revert to `lowlighter/metrics`?**

The fork PR (lowlighter/metrics#1769) has been open since Nov 2025, last updated Jan 2026, and is `mergeable: true` but unmerged. Verified 2026-09-30: still open and unmerged. Answer: treat the fork as semi-permanent — it stays SHA-pinned (9ea8677) with periodic checks (TODO_LIST T3); revert to upstream and re-pin only when #1769 merges.

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
