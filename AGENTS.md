# LarsArtmann/LarsArtmann — GitHub Profile Repository

**What this is**: The special repository that renders the profile README at [github.com/LarsArtmann](https://github.com/LarsArtmann). It is a sales surface, not a code project. The actual website lives in [larsartmann.com](https://github.com/LarsArtmann/larsartmann.com) — **private repo** (link checkers like lychee report 404 for it; that is expected, not a broken link), Astro, deployed to Firebase Hosting — and its `/projects` page is the live proof page this profile links to.

## Layout

| Path                            | Role                                                                                                             |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `README.md`                     | The profile sales page. The only file rendered on the GitHub profile.                                            |
| `.github/workflows/metrics.yml` | Metrics + trophy SVG generation. **Manual-only** (`workflow_dispatch`); auto-triggers are deliberately disabled. |
| `.github/dependabot.yml`        | Weekly grouped bumps for GitHub Actions.                                                                         |
| `SETUP.md`                      | `METRICS_TOKEN` setup and workflow runbook.                                                                      |
| `docs/status/`                  | Session status reports. Resolved ones are annotated inline (strikethrough) and moved to `docs/status/archived/`. |
| `assets/`                       | README imagery (iSAQB CPSA-F logo).                                                                              |

## Gotchas

- The three SVGs (`metrics.svg`, `metrics.repositories.svg`, `trophies.svg`) are generated artifacts that are currently **not referenced** by `README.md`. Do not "fix" the README to display them without first deciding to reintroduce live metrics (see TODO_LIST T5).
- `Erik-Donath/github-profile-trophy` writes `trophy.svg` regardless of its `file` input; the workflow papers over this with `cp trophy.svg trophies.svg 2>/dev/null || true`, which silently swallows errors. Known wart, tracked in TODO_LIST T1.
- Every `uses:` is pinned to a commit SHA (supply-chain rule). Dependabot opens weekly grouped PRs against those pins.
- Metrics run on the `dkhokhlov/metrics` fork (Projects V2 migration) because upstream [lowlighter/metrics#1769](https://github.com/lowlighter/metrics/pull/1769) was still unmerged as of 2026-09-30. If it merges, switch back to upstream and pin its SHA (TODO_LIST T3).
- `METRICS_TOKEN` is a classic PAT with `repo`, `read:user`, `read:org`, `read:project` scope. Replacing it with a fine-grained, repo-scoped PAT is TODO_LIST T2.
- Quality automation runs through **BuildFlow** (`buildflow format` / `buildflow --build-mode lightning`) — it owns dprint formatting (`dprint.json`), the gitignore block, and the link check (lychee). There is no build system, no tests, nothing to compile — `nix flake check` does not apply here.
- An auto-commit daemon commits working-tree changes continuously. Expect commits you did not make; never fight them with resets.

## Working on the sales page

- Feature claims in `README.md` must be verifiable: every linked repository should exist, be pushed recently, and match its one-liner. Star counts are deliberately **not** hardcoded (they rot); relative proofs like "adopted by Swiss Post" are.
- The canonical numbers, install commands, and project curation live on larsartmann.com `/projects` (resolved from the GitHub API at build time). Keep the profile consistent with it — do not duplicate numbers here.
