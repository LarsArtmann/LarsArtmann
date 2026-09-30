# Roadmap

Long-term direction and raw ideas. Nothing here is committed; ideas graduate to `TODO_LIST.md` when they become bounded and actionable.

## Profile evolution

- **Reintroduce live metrics** — `schedule` trigger plus `<picture>` embeds with light/dark SVG variants. Only worth doing after TODO_LIST T1 (trophy output) and T3 (fork dependency) land; otherwise automation amplifies a broken pipeline. _(from 2026-05-02 report C1/F3/F10)_
- **GitHub Wrapped section** — the profile once showed 2022–2024 wrapped images; 2025 was never added. Only relevant if dynamic sections return. _(report C2/F13)_
- **WakaBox coding-time stats** — daily coding-activity card. _(report F18)_
- **GitHub Skyline 3D contribution graph** — visual flourish; static image would need periodic regeneration. _(report F19)_
- **Self-host github-profile-trophy on Vercel** — removes the last third-party fork dependency for trophies; high effort, low urgency while metrics are manual-only. _(report F24)_

## Workflow polish (when metrics return)

- `retries: 3` on the metrics action for transient API failures. _(report F16)_
- SVG validity check before committing generated artifacts. _(report F17)_
- `workflow_dispatch` inputs for manual force-refresh of individual SVGs. _(report F21)_
