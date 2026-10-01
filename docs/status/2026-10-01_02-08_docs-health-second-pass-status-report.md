# Status Report — 2026-10-01 02:08 — Docs-Health Second Pass (VERIFY + HARVEST + Fixes)

**Session scope:** second docs-health pass on `LarsArtmann/LarsArtmann`, continuation of the two 2026-09-30 passes (04:33 baseline, 07:19 reshape). This stretch ran ~2026-09-30 07:30–08:20 CEST (work) and this report was written 2026-10-01 02:08 CEST (session resumed overnight). Covers: full AUDIT (BUILD + HARVEST + VERIFY) over every file in the repo, external claim verification, all in-repo fixes, and the quality gate. First audit of this stretch — no separate baseline beyond this morning's two reports.

**Scope discipline (per instruction):** no research beyond what this session touched and noticed.

---

## 0) Self-Critique: What I Forgot, What Could Be Better, What Could Still Improve

| # | Miss / gap | Consequence | Disposition |
| - | ---------- | ----------- | ----------- |
| 1 | **Did not check larsartmann.com for the same CodersRank "Germany" claim.** I corrected the README to Switzerland-based facts, but the site may carry the identical claim — meaning my fix may have *created* a profile↔site divergence, the exact failure mode this repo's own rules warn about. The check is a 1-minute fetch | Possible new inconsistency introduced by the fix itself | Not done (different repo; flagged as §f item 3) |
| 2 | **Accepted the freshly merged Dependabot bump (`cb20fa8`, actions/checkout v4 → v7.0.1) without verifying its pin.** Every other `uses:` SHA in this repo was verified against upstream in prior passes; the new pin (`3d3c42e…`) entered via auto-merge and I pulled it without re-applying the same supply-chain check — to a *major*-version jump, no less | One unverified third-party pin in the workflow that runs with `METRICS_TOKEN` | Routed as §f item 1 (S effort) |
| 3 | **Left the two same-day reports unannotated even though this pass resolved their open questions** — §g Q2 (CodersRank) of both reports is now answered, §f2 (push) went stale again within hours. ANNOTATE was outside AUDIT scope and no file range was specified, so skipping was defensible — but the CodersRank resolution is exactly the high-value annotation case, and I chose cleanliness over completeness | Readers of the 07:19 report still see CodersRank as "unverifiable, awaiting Lars" when it is verified and fixed | Deliberate; routed as §f item 25 |
| 4 | **"All metrics are generated from public data only" (`SETUP.md:51`) left unverified.** Workflow semantics (`repositories_affiliations`, plugin filters) can't be statically proven to exclude private data; I flagged it in the health report but routed it nowhere | An inherited claim survives unaudited | Routed as §f item 17 |
| 5 | **Skipped BuildFlow's step-1 discipline** (`--dry-run --verbose` to see what applies) and went straight to `buildflow format`. It worked — but that is luck-adjacent, not process | None this time; could mask a silently-skipped tool next time | Process note (§e-5) |
| 6 | **Kept the emeet-pixyd "PTZ" claim on inherited evidence** — the official description says "face tracking", not PTZ; the PTZ attribution comes from the 07:19 report's topic check, which I did not repeat | Trivial accuracy risk on one word | Accepted (same-day verification existed); noted |
| 7 | **The featured-list sync ("if metrics return, sync `plugin_repositories_featured` to the README five") is routed only implicitly via T5 execution** — `metrics.yml:62` still lists the dropped set (dynamic-markdown-site, go-filewatcher, art-dupl, go-branded-id), and I didn't land it explicitly in ROADMAP | Conditional item lives in prose, not in the doc that owns conditionals | Routed as §f item 31 |
| 8 | **Added more line-number citations** (README.md:38, :39-41, :53-54; metrics.yml:81…) — the same convenience-rot pattern the repo already carries; they break on every upstream edit | Citation maintenance tax on every future pass | Accepted tradeoff (§e-6) |
| 9 | **Did not investigate `.config/metadata.yaml` `updated_at` semantics** (still 2026-09-10 despite today's doc rewrites) — unknown whether anything is supposed to bump it | Possible stale field nobody owns | Routed as §f item 47 |

**What went well:** the CodersRank claim was killed by primary-source verification done *twice* (agentic fetch + direct fetch agreed) instead of being parked as "owner-only" for a third pass; the About/pins blind spot that two prior sessions flagged as uninspected is now inspected with concrete API evidence; the fix set was executed end-to-end and gated green (`buildflow format` 16 success / 0 failed); zero history rewritten; nothing pushed.

---

## a) FULLY DONE

| # | Item | Evidence |
| - | ------ | -------- |
| 1 | **Critical claim corrected: CodersRank.** The README proof line "Top 1% globally. Top 50 in Germany (Kotlin, Java)" is contradicted by the public profile — verified twice against `profile.codersrank.io/user/LarsArtmann` (agentic + direct fetch, both 2026-09-30): location **Zuerich, Switzerland**; overall **Top 1%** (rank 5 CH); Kotlin top 10% of 27K worldwide; Java top 9% of 131K; no German ranking visible. README now reads: "Top 1% globally; Kotlin and Java top 10% worldwide (2026-09)" | `README.md:51`; commits `d05613f` (via daemon), `5e81c33` |
| 2 | **emeet-pixyd one-liner now names both camera models** — "EMEET PIXY and PIXY 2K", matching the official repo description fetched via API (07:19 report §f7 executed) | `README.md:41`; GitHub API description 2026-09-30 |
| 3 | **Profile↔site curation-split rule codified (profile side)** — AGENTS.md now states the two lists intentionally differ (profile = proof + identity incl. proprietary; site = installable open source) and must not be mechanically synced (07:19 report §f5, in-repo half) | `AGENTS.md` (Working on the sales page) |
| 4 | **SETUP.md token scopes fixed** — `read:project` was missing (the achievements plugin on the `dkhokhlov/metrics` fork reads Projects V2), the `repo`-scope reality is now documented with a T2 pointer, and the troubleshooting scope list was corrected. The old instructions would have re-created the exact achievements breakage the fork exists to fix | `SETUP.md` (Required Setup §1, Troubleshooting) |
| 5 | **Phantom streak-stats self-hosting advice removed** — the profile uses no streak-stats anywhere (README, workflow); the block was leftover drift despite the 04:33 pass claiming such services were removed | `SETUP.md` (Self-Hosting: only Trophies remains); `grep -c streak` → 0 |
| 6 | **TODO_LIST T4 executed** — Go-specific `.gitignore` template block stripped (binaries, `*.test`, `*.out`, `vendor/`, `node_modules/`, `go.work`); buildflow-managed block untouched | `.gitignore` (now starts at "Build directories"); `buildflow format` gitignore-upserter verify ✔ |
| 7 | **TODO_LIST rebuilt via HARVEST** — completed T4 removed (logged in CHANGELOG, never kept in the list); T6–T12 harvested from both same-day status reports with why + evidence + owner status; source line updated | `TODO_LIST.md` (T1–T3, T5–T12) |
| 8 | **FEATURES.md honest inventory extended** — new BROKEN row "Repo About surface" (GitHub API 2026-09-30: `description: null`, `topics: []`; pinned repos ≠ README five — only typespec-asyncapi overlaps); counts recomputed: 5 FULLY_FUNCTIONAL · 3 PARTIALLY_FUNCTIONAL · 1 BROKEN · 1 DISABLED · 0 PLANNED | `FEATURES.md`; `gh api repos/LarsArtmann/LarsArtmann`; GraphQL `pinnedItems` |
| 9 | **ROADMAP gained the three unrouted raw ideas** — capsule-render SPOF replacement, deep-linking showcase rows to site cards, German README variant | `ROADMAP.md` (Profile evolution) |
| 10 | **CHANGELOG appended (append-only respected)** — seven new bullets under the existing 2026-09-30 heading; no prior entries touched | `CHANGELOG.md` |
| 11 | **Archived report pointer de-rotted** — "(T1–T5)" range reference (already stale the moment T6+ appeared) generalized to a range-free pointer; annotation completeness gate still passes (`grep -rLn '~~' docs/status/archived/` → empty) | `docs/status/archived/2026-05-02_22-57_session-retrospective.md:3` |
| 12 | **Remote state reconciled** — fetched and fast-forwarded `cb20fa8` (Dependabot: actions/checkout bump, merged remotely); confirmed all cited `metrics.yml` line numbers survived the 1:1 line replacement | `git log`; `grep -n 'uses:\|cp trophy' .github/workflows/metrics.yml` |
| 13 | **External claims re-verified via GitHub API** — all five showcase repos exist, public, pushed <24 h (typespec 17★ MIT; go-cqrs-lite NOASSERTION; cqrs-htmx MIT; templ-components MIT; emeet-pixyd MIT); "97 components" matches the live repo description; lowlighter/metrics#1769 still open/unmerged (T3 current); SETUP.md's "Issue #13" reference is a real open issue | `gh api` × 9; `gh api graphql` pinnedItems |
| 14 | **Quality gate green** — `buildflow format`: 16 steps success / 0 failed; dprint formatted 3 files (table realignment); lychee 26 links OK, only the 2 documented false positives (private-repo 404, LinkedIn 999); link total dropped 29→28 with the streak-stats removal, as expected | buildflow output 08:19:59–08:20:01 |
| 15 | **Health report computed with visible math** — Accuracy 7.0 / Fitness 8.5 from the findings table (2 Critical, 1 Medium, 2 Low, 2 Medium-High); every fixable finding fixed in-session, owner-gated items routed | Inline health report, 2026-09-30 ~08:20 |
| 16 | **All edits verified persisted** through the auto-commit daemon (`d05613f` 08:18: AGENTS/SETUP/.gitignore; `5e81c33` 08:20: the rest) — per the BuildFlow skill's "don't trust the daemon" rule | `git show` × 2; `grep` spot-checks × 4 |

## b) PARTIALLY DONE

| # | Item | Works | Missing | Blocker | Effort |
| - | ---- | ----- | ------- | ------- | ------ |
| 1 | **Profile↔site curation split** | Profile-side rule codified in AGENTS.md; split understood and documented on this side | Site-side half: the one-sentence statement in the site repo + a deliberate decision on emeet-pixyd/go-cqrs-lite site curation; plus — new since the CodersRank fix — whether the site mirrors the corrected rank claim | Different repo, own gated process | S (sentence) + M (site change) |
| 2 | **Repo About surface + pins (T6)** | Fully audited with API evidence; concrete proposal written into T6 (description text, homepage, re-pin to the README five + 1) | The actual GitHub-settings change — description/homepage/topics/pins are all still empty/pre-reshape on github.com right now | Owner-only settings (or explicit go-ahead to apply via `gh repo edit` / GraphQL) | S |
| 3 | **README push** | Everything through 07:22 (+ remote `cb20fa8`) is on github.com — the reshape IS live | This pass's commits (`d05613f`, `5e81c33` + this report) are local-only until a manual push | Push is Lars's action (never auto) | S |
| 4 | **CodersRank claim** | Corrected to twice-verified public facts, date-stamped (2026-09); the contradicted Germany claim is gone | Final wording/placement is a positioning call; the location question (Zuerich vs the old Germany claim) is unanswered — §g Q3; site mirror unchecked | Lars's call (§g Q3) | S |
| 5 | **Carried TODOs T1/T2/T3/T5** | All four accurately documented with evidence; T3 re-verified current today | Unchanged — see TODO_LIST | T1/T2/T5 need Lars (settings/decision); T3 blocked on upstream | M/S |

## c) NOT STARTED

| # | Item | Why not started | Still wanted? |
| - | ---- | --------------- | ------------- |
| 1 | T6 application (About/pins settings change) | Owner-only; proposal ready | Yes — highest visible win |
| 2 | T7 `(proprietary)` framing decision | Positioning call, owner-only | Yes |
| 3 | T8 docs-site links in showcase rows | Row-format design decision (one link per row today) | Yes |
| 4 | T9 quarterly docs-health pass scheduling | Calendar action, owner-side | Yes |
| 5 | T10 lychee excludes + minimal `.buildflow.yml` | Bounded, ready; not needed for this pass's gate run | Yes |
| 6 | T11 quarterly re-verification script | M-effort tooling; this pass did its job manually | Yes |
| 7 | T12 clients + 2021 speaker line re-justification | Only Lars knows currency/permission | Yes |
| 8 | Verify the merged actions/checkout v7.0.1 pin against upstream (§f1) | Discovered at the end of the pass; deliberate deferral to keep scope | Yes — security-relevant |
| 9 | Site-side CodersRank/Germany consistency check (§f3) | Different repo; session scope stopped at the profile | Yes |
| 10 | "Public data only" SETUP.md claim verification (§f17) | Needs workflow-semantics research beyond this pass | Yes |
| 11 | Annotating the two same-day reports for items this pass resolved (§f25) | ANNOTATE range not specified; same-day snapshots left frozen by design | Optional |
| 12 | `metadata.yaml` `updated_at` semantics (§f47) | Unknown ownership; not blocking | Maybe |

## d) TOTALLY FUCKED UP

| # | Item | Severity | Root cause | Mitigation |
| - | ---- | -------- | ---------- | ---------- |
| 1 | **A contradicted rank claim sat in the flagship proof section through TWO docs-health passes today.** "Top 50 in Germany (Kotlin, Java)" was flagged as "unverifiable without Lars's account" at 04:33 and re-carried as an open question at 07:19 — when the public profile was fetchable all along and directly contradicts it (Switzerland-based ranks). A sales page saying something the primary source disproves is worse than no claim | High (credibility — on the most visible page) | Verification stopped at "needs the owner" without testing whether the primary source is public; two passes institutionalized the blind spot instead of catching it | **Fixed this pass**; lesson codified in §e-1; date-stamped so the next pass re-checks |
| 2 | **The repo About surface has been blank while the README got two polish passes.** `description: null`, `topics: []`, homepage unset; the six pinned repos still mirror the pre-reshape curation (gogenfilter, art-dupl, go-output, segment-buffer, SystemNix — only typespec-asyncapi overlaps). The About line and pins are the first thing any profile visitor sees | High (sales) — invisible flagship work | Both prior passes scoped "sales surface" to README.md and deferred GitHub-settings surfaces as owner-only *without even inspecting them*; uninspected became unaudited | Inspected this pass; BROKEN row in FEATURES; proposal ready in T6; application is one owner action |
| 3 | **SETUP.md's setup instructions reproduced a known-broken token.** The scope list (read:user, read:org, user:email) omits `read:project` — required by the achievements plugin on the fork — and omits any write scope although the workflow checks out and pushes with the token. Anyone re-provisioning per the runbook recreates the exact breakage the fork migration fixed | Medium (triggers only on re-setup) | Scope needs drifted when the fork was adopted (2026-05); the 04:33 "SETUP.md drift fixed" pass corrected services and display claims but never re-checked scopes against the actual token | **Fixed this pass**; scopes now match the documented token; T2 will shrink them properly |
| 4 | **Supply-chain verification has a hole exactly where automation is trusted most.** Every `uses:` SHA was hand-verified in prior passes, but the Dependabot bump merged remotely (`cb20fa8`, checkout v4→v7.0.1 — a major jump) was pulled into this session without verifying the new SHA against the upstream action | Medium (latent; token-consuming workflow) | Auto-merge was treated as reviewed; "Dependabot pinned it" substituted for the verification rule that exists for everything else | Routed §f1 (S); rule proposed in §e-3 |
| 5 | **Carried, still real, still unfixed:** trophy `cp … \|\| true` swallow (T1); classic PAT with full `repo` scope consumed by a third-party fork (T2); the fork dependency itself (T3, upstream still unmerged — re-verified today); the dead SVG trio + manual-only workflow (T5) | M/H (as previously assessed) | see 04:33 §d | TODO_LIST T1–T3, T5 |

## e) WHAT WE SHOULD IMPROVE

1. **"Unverifiable without the owner" is a hypothesis, not a verdict.** The CodersRank claim survived two passes as a §g question; one fetch of the public profile killed it. New rule for every pass: before parking a claim as owner-only, attempt the public primary source once (profile pages, public APIs, public package registries).
2. **Sales-surface audits must enumerate GitHub-settings surfaces**, not just files. About, topics, homepage, social preview, and pinned repos are part of the profile; inventory them every pass even when the fix is owner-gated — uninspected surfaces accumulate the exact drift that just surfaced (§d-2).
3. **Auto-merged Dependabot bumps get the same pin verification as hand-edits.** The grouped auto-merge is convenient precisely because nobody looks — which is why the SHA check must be explicit after each merge (major-version jumps especially).
4. **Resolve-then-annotate, same session.** When a pass answers a report's open question (as this one answered §g Q2), annotate that item inline immediately — the 07:19 report's own §e6 allows same-session annotation, and unannotated "open" questions mislead every later reader.
5. **Follow the BuildFlow loop's step 1** (`--dry-run --verbose`) even when the final command is predictable — it is the difference between "green" and "green and actually scanned".
6. **Prefer section anchors over bare line numbers** where the target doc allows it, or accept that every pass pays a citation-repair tax (today: verified all `metrics.yml` cites survived only because the bump was a 1:1 line replacement — that was luck).
7. **Two-source verification for external claims works** — agentic fetch and direct fetch agreeing is what made the CodersRank correction safe to ship. Keep the pattern for any claim that will be date-stamped into the README.
8. **Proof-claim consistency belongs in the profile↔site check.** The documented consistency rule covers curation lists; the CodersRank fix shows *claims* (rankings, client names, percentages) can diverge too — add a claims diff to the quarterly pass (fold into T9/T11 scope).

## f) Top 50 Things to Get Done Next

_New/updated items first (N1–N7); carried items follow (numbering continuous). Impact-sorted within each group. **[T#]/[RM]** = already routed in TODO_LIST/ROADMAP. Survivors not yet routed (pending next HARVEST, per the wait instruction): N1, N3, N17, N18, N25, N47._

| # | Task | Impact | Effort | Category |
| - | ---- | ------ | ------ | -------- |
| 1 | Verify the merged actions/checkout pin (v4→v7.0.1, `cb20fa8`): confirm SHA `3d3c42e…` is upstream checkout and v7 runs on ubuntu-latest — the one `uses:` that entered unreviewed | HIGH | S | Security [NEW] |
| 2 | Push this pass's local commits (`d05613f`, `5e81c33`, + this report) so github.com shows the corrected claims | HIGH | S | Ship |
| 3 | Check larsartmann.com for the same CodersRank "Germany" claim and sync — the README fix may have introduced a profile↔site divergence | HIGH | S | Consistency [NEW] |
| 4 | T6: apply the About/pins proposal (description, homepage `https://larsartmann.com`, topics, re-pin to the README five + 1) | HIGH | S | Presentation [T6] |
| 5 | T5 decision: delete the three SVGs + manual-only workflow, or reintroduce live metrics — still gates ~6 roadmap items and SETUP.md's purpose | HIGH | S | Decision [T5] |
| 6 | T7: confirm the `(proprietary)` framing for go-cqrs-lite (marker wording/placement vs alternatives) | HIGH | S | Decision [T7] |
| 7 | T2: replace `METRICS_TOKEN` with a fine-grained, repo-scoped PAT | HIGH | M | Security [T2] |
| 8 | T1: verify trophy `file` input, drop the `cp … \|\| true` swallow | HIGH | M | Reliability [T1] |
| 9 | T3: watch lowlighter/metrics#1769 (re-verified open 2026-09-30); revert to upstream + re-pin on merge | HIGH | S | Security [T3] |
| 10 | larsartmann.com: push + merge `rebuild/brutal-clarity` (6 commits), fix CI billing, manual deploy | HIGH | M | Site |
| 11 | T9: schedule the quarterly docs-health pass (next ~2026-12); scope: re-verify README claims ("97 components", CodersRank date-stamp, licenses) + profile↔site library AND proof-claim diff | MEDIUM | S | Process [T9] |
| 12 | T8: link the docs sites in the showcase rows (templcomponents.lars.software, emeet-pixyd.lars.software) | MEDIUM | S | Presentation [T8] |
| 13 | T12: re-justify clients (Hornbach, NOBLETARY) currency/permission + the 2021 speaker line vs newer proof | MEDIUM | S | Accuracy [T12] |
| 14 | Audit the repo social preview image — the one About-surface item still uninspected | MEDIUM | S | Presentation |
| 15 | T10: lychee excludes (private-repo 404, LinkedIn 999) + minimal `.buildflow.yml` for a clean-by-default gate | MEDIUM | S | Tooling [T10] |
| 16 | T11: quarterly re-verification script — `gh api` existence + push recency + license for every README-linked repo | MEDIUM | M | Tooling [T11] |
| 17 | Verify SETUP.md's "all metrics generated from public data only" against actual workflow semantics | MEDIUM | S | Accuracy [NEW] |
| 18 | Decide the CodersRank story: location (Zuerich vs the old Germany claim), keep/drop, site mirror (§g Q3) | MEDIUM | S | Accuracy [NEW] |
| 19 | Render-test the README via GitHub's HTML render endpoint after edits (catch table/HTML breakage) | MEDIUM | S | QA |
| 20 | Copywriting pass on README (hook, proof order, CTA) | MEDIUM | M | Presentation |
| 21 | Cross-link the Swiss Post case study when it exists (site TODO #53) | MEDIUM | S | Site |
| 22 | Document METRICS_TOKEN scopes per plugin (feeds T2; SETUP now names the four real scopes — verify per-plugin minimums) | MEDIUM | S | Security |
| 23 | Deep-link showcase rows to the site's per-project cards [RM] | MEDIUM | S | Presentation |
| 24 | Site curation proposal for emeet-pixyd + deliberate rule for go-cqrs-lite (site-side half of the split sentence) | MEDIUM | S | Site |
| 25 | Annotate the two 2026-09-30 reports for items this pass resolved (§g Q2 CodersRank, §f2 push, §f7 PIXY 2K, §f5-profile) | LOW | S | Docs [NEW] |
| 26 | "Last verified" HTML comment in README for freshness audits | LOW | S | Process |
| 27 | iSAQB logo attribution/license note | LOW | S | Hygiene |
| 28 | CHANGELOG badge/link in README | LOW | S | Presentation |
| 29 | Prune ROADMAP items T5 renders moot, immediately after the decision | LOW | S | Hygiene |
| 30 | Capture date evidence for the three SVG artifacts' last real regeneration (feeds T5) | LOW | S | Evidence |
| 31 | Verify trophies.svg content actually matches the current trophy config | LOW | S | QA |
| 32 | If metrics return: sync `plugin_repositories_featured` (metrics.yml:62) to the README five — it still lists the dropped set | MEDIUM | S | Consistency |
| 33 | If metrics return: light/dark SVG variants [RM] | LOW | M | Presentation |
| 34 | If metrics return: schedule trigger + self-trigger guard re-check [RM] | MEDIUM | S | Reliability |
| 35 | If metrics return: `retries: 3` on the metrics action [RM] | LOW | S | Reliability |
| 36 | If metrics return: SVG validity check before commit [RM] | LOW | M | Reliability |
| 37 | If metrics return: `workflow_dispatch` inputs for per-SVG force-refresh [RM] | LOW | S | DX |
| 38 | WakaBox coding-time stats [RM] | LOW | M | Presentation |
| 39 | GitHub Skyline 3D contribution graph [RM] | LOW | M | Presentation |
| 40 | Self-host github-profile-trophy on Vercel [RM] | LOW | L | Architecture |
| 41 | GitHub Wrapped 2025 section [RM] | LOW | S | Presentation |
| 42 | Replace capsule-render banners with committed SVGs (kill the vercel.app SPOF) [RM] | LOW | S | Reliability |
| 43 | German README variant mirroring the site's EN/DE parity [RM] | LOW | M | Presentation |
| 44 | Evaluate dependabot tracking for the pinned `dkhokhlov/metrics` fork SHA (currently forever-stale) | LOW | S | Security |
| 45 | Dependabot merge-cadence note — weekly grouped PRs only matter if they get merged | LOW | S | Process |
| 46 | Check `git-town.toml` still matches the current workflow | LOW | S | Hygiene |
| 47 | Investigate `.config/metadata.yaml` `updated_at` semantics (2026-09-10, predates today's doc rewrites — what owns it?) | LOW | S | Hygiene [NEW] |
| 48 | Decide "97 components" policy (recheck per pass vs soften to "90+") — folded into T9's checklist | LOW | S | Accuracy |
| 49 | Quarterly recheck of the CodersRank numbers (date-stamped 2026-09 in README) — folded into T9 | LOW | S | Accuracy |
| 50 | Add a docs/status index OR consciously document its absence in AGENTS.md (two open + one archived report currently have no index) | LOW | S | Docs |

## g) Top 3 Questions I Cannot Figure Out Myself

1. **T5, still the biggest unblocker:** delete the three metrics SVGs and accept a manual-only workflow, or reintroduce live metrics properly (schedule trigger, light/dark variants, featured-list synced to the README five)? I re-verified the mechanics this pass (workflow, artifacts, SETUP runbook) and the tradeoffs are unchanged; the sales-strategy call is yours. It gates items 5, 29–37 above and most of ROADMAP.

2. **T7: how should proprietary work read on the sales page?** go-cqrs-lite carries the blunt `(proprietary)` marker — my predecessor session's unilateral call, which I deliberately did not second-guess this pass. Blunt marker, softer "in production" signal, separate frameworks line, or no marker with only the intro carrying the nuance? I verified the license (PROPRIETARY, GitHub `NOASSERTION`); only you can pick the story it tells.

3. **CodersRank: which country is the truth, and what should the page claim?** The public profile is set to **Zuerich, Switzerland** and shows Switzerland-based badges (Kotlin #1 CH, Nix top 5 CH, TypeScript top 6% CH) — while the README claimed "Top 50 in Germany (Kotlin, Java)" until I replaced it today with the verified Switzerland-based facts. I cannot see your account's location history: Was the Germany claim true under an old location setting, and did you intentionally switch CodersRank to Zuerich? And do you want the Switzerland-based proof at all (Kotlin #1 in CH is a tiny pool of 22; the worldwide percentages are the stronger, safer claim)? Your answer decides the README line, whether the site mirrors it, and what the quarterly pass re-checks.

---

## Session Timeline (this stretch)

| Time (CEST) | What Happened |
| ----------- | ------------- |
| ~07:30 | Loaded docs-health skill + 6 references; inventoried and read every file in the repo (living docs, workflow, configs, all 3 status reports) |
| ~07:35 | Fetched remote; fast-forwarded `cb20fa8` (Dependabot checkout bump); re-verified all cited `metrics.yml` line numbers survived |
| ~07:40 | External verification sweep: 5 showcase repos (license/recency/description), PR 1769 state, repo About (`null`/`[]`), pinned repos (GraphQL), issue #13 |
| ~07:50 | CodersRank investigation: agentic fetch → contradiction found → direct fetch of the profile confirmed (twice) |
| ~08:05 | Fixes: README (CodersRank, PIXY 2K), AGENTS.md (curation split), SETUP.md (scopes ×2, streak-stats removal), `.gitignore` (T4), TODO_LIST (T4 removed, T6–T12 harvested), CHANGELOG (append), FEATURES (BROKEN row + counts), ROADMAP (+3 ideas), archived pointer |
| 08:18–08:20 | Auto-daemon committed all edits (`d05613f`, `5e81c33`); edits spot-verified persisted |
| 08:19–08:20 | `buildflow format` green: 16/0, dprint ×3, lychee 26 OK + 2 documented false positives |
| ~08:20 | Health report printed inline (Accuracy 7.0 / Fitness 8.5, visible math) |
| 02:08 (+1d) | This report written (session resumed overnight) |

**Format note:** the status-report skill's canonical output is a styled HTML dashboard; this report is Markdown because the request explicitly specified a `.md` path. One-off override — not propagated anywhere.
