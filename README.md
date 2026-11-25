# HISPEC RV Recovery

Static documentation of radial-velocity (RV) recovery tests performed with
[`HISPEC_DRP`](https://github.com/charliekilpatrick/HISPEC_DRP). This repository
is **results and narrative only** — markdown, tables, and plots — so readers can
inspect outcomes without running the pipeline.

The code that produced these results lives in the sibling `HISPEC_DRP` checkout.
Open both folders together via the multi-root Cursor/VS Code workspace
[`HISPEC_DRP_and_RV_Recovery.code-workspace`](../HISPEC_DRP_and_RV_Recovery.code-workspace)
(one level up from this repo).

## Contents

| Page | Topic |
|------|--------|
| [docs/00_overview.md](docs/00_overview.md) | Scope, products, how tests map to `HISPEC_DRP` |
| [docs/01_multi_line_centroid_ccf.md](docs/01_multi_line_centroid_ccf.md) | Multi-line CCF Δ bias / quality cuts |
| [docs/02_rms_over_sqrt_n_optimize.md](docs/02_rms_over_sqrt_n_optimize.md) | Full-parent iterative rezero + RMS/√N floor |

## Layout

```text
docs/                 Markdown write-ups
assets/plots/         Embedded figures
assets/tables/        JSON / CSV summaries
assets/masks/         Selected .mas products referenced in the docs
```

## Local Cursor setup

1. Keep checkouts as siblings:
   - `…/cursor/HISPEC_DRP`
   - `…/cursor/HISPEC_RV_Recovery`
2. Open `…/cursor/HISPEC_DRP_and_RV_Recovery.code-workspace` in Cursor
   (**File → Open Workspace from File…**).
3. Both roots appear in the explorer; Agent/chat can read either tree.
