# Changelog

All notable changes to this repository, newest first. This file was started 2026-09-30; earlier entries are backfilled from git history. Append-only: never edit prior entries.

## 2026-09-30

- Documentation health pass: rewrote `AGENTS.md` as repo-specific context (it previously duplicated the global engineering philosophy and pointed at nonexistent `references/` files), added `FEATURES.md`, `TODO_LIST.md`, `ROADMAP.md`, and this `CHANGELOG.md`, corrected `SETUP.md` drift, featured `typespec-asyncapi` in the README, and annotated + archived the 2026-05-02 session report to `docs/status/archived/`.
- README showcase reshaped around proof + identity: now features typespec-asyncapi, go-cqrs-lite (proprietary, labeled as such), cqrs-htmx, templ-components, and emeet-pixyd. The previous three 1★ primitive rows (go-branded-id, go-composable-business-types, cmdguard) were dropped from the table — they remain curated on larsartmann.com/projects.
- GitHub Actions pinned to commit SHAs; Dependabot added for weekly grouped action updates; metrics workflow auto-triggers disabled (`9ea8677`).

## 2026-09-10

- Normalized `.config/metadata.yaml` to the canonical schema (`0fda882`).

## 2026-09-04

- README links the live proof page at larsartmann.com/projects (`21823fd`).

## 2026-07-21

- Metrics switched to public-only data; library showcase trimmed (`53e0786`).

## 2026-07-09

- README simplified into a focused sales page; metadata normalized (`d68c405`).
- Official iSAQB CPSA-F certification logo added (`628d9b6`).
- Dark/light theme support for the banner images (`087c387`).
- Removed link to the non-public SEC repository (`9e1c616`).

## 2026-05-02

- Achievements plugin fixed by migrating to the Projects V2 API (`dkhokhlov/metrics` fork + `read:project` scope).
- Trophy generation moved in-workflow (SVG committed by the action itself, includes private-repo data).
- Featured repositories refreshed to dynamic-markdown-site, go-filewatcher, art-dupl, emeet-pixyd, go-branded-id.
- Session report written to `docs/status/` (archived 2026-09-30).

## 2026-02-16

- Metrics, trophies, and stats introduced to the profile (`e169e67`).

## 2021-10-05

- Repository created (`9f4d88c`).
