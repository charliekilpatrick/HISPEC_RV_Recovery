# Full-parent iterative rezero + RMS/√N optimization

## How it was run (`HISPEC_DRP`)

Script: `examples/scripts/scratch/optimize_mask_rms_over_sqrt_n.py`

1. **Seed** — packaged gradient parent  
   `hispecdrp/data/masks/lte040…delta.mas` (**6857** lines; \(dF/d\lambda=0\), \(d^2F/d\lambda^2>0\)).
2. **Paint** — ideal L2s for inject \(0\)–\(10\) km/s @ 1 km/s (`n_fine=512`).
3. **Iterative ccfopt rezero** — `iteratively_rezero_ccfopt_line_mask` until
   \(\max|\mathrm{RV}-\mathrm{center}| < 0.001\) m/s (0.1 cm/s).
4. **Features** — per-line \(\Delta\), bias, \(RV_{ERR}\), etc. on the rezeroed mask.
5. **Optimize** — small MLP keep-scorer + grid over \(\Delta\) cut, \(|\mathrm{RV}-v|\) gate,
   \(|\mathrm{bias}_0|\) gate; minimize \(\mathrm{RMS}(\Delta)/\sqrt{N}\).

Machine-readable summary:
[`assets/tables/rms_n_optimize_summary.json`](../assets/tables/rms_n_optimize_summary.json).

Masks (snapshots):

- [`assets/masks/lte040.parent_iter_rezero.mas`](../assets/masks/lte040.parent_iter_rezero.mas) — after rezero
- [`assets/masks/lte040.ccfopt.rms_n_optimized.mas`](../assets/masks/lte040.ccfopt.rms_n_optimized.mas) — RMS/√N floor product

## Line counts through the funnel

| Stage | \(N\) |
|-------|------:|
| Gradient parent (full template band) | 6857 |
| After iterative rezero (BSPEC on-band, verify pass) | 738 |
| Match drops (off-order / no chunk) during rezero | 6009 |
| Alias drops | 63 |
| Verify drops | 47 |
| Seed “good” labels (\(\max|\Delta|\le10\), \(\max|\mathrm{bias}|\le1\) km/s) | 560 |

Final rezero residual: \(\max|\mathrm{RV}-\mathrm{center}| \approx 9.7\times10^{-4}\) m/s
(8 iterations).

## Baselines on the rezeroed pool (\(N=738\))

| Selection | \(N\) | RMS (m/s) | RMS/√N (m/s) |
|-----------|------:|----------:|-------------:|
| All finite | 738 | 706.4 | 26.00 |
| \(\Delta\le20\) | 687 | 4.40 | 0.168 |
| \(\Delta\le10\) | 560 | 2.86 | 0.121 |
| \(\Delta\le5\) | 394 | 1.56 | 0.0785 |
| **Optimized floor** | **276** | **1.022** | **0.0615** |

## Floor (objective minimum)

| Quantity | Value |
|----------|------:|
| \(N\) | **276** |
| RMS(\(\Delta\)) | **1.022 m/s** |
| **RMS / √N** | **0.0615 m/s** |
| \(\Delta\) cut | 3.0 m/s |
| \(\|\mathrm{RV}-v\|\) gate | 200 m/s |
| \(\|\mathrm{bias}_0\|\) gate | 1.0 m/s |

On this dataset the MLP did not beat a pure gate sweep (same floor). Lowering
\(\mathrm{RMS}/\sqrt{N}\) relative to the Δ≤10 baseline trades sample size
(\(560\to276\)) for a cleaner scatter (\(2.86\to1.02\) m/s), improving the
aggregate mean-RV scale by about \(2\times\).

## Relation to earlier Δ-cut work

Prior notebook quality work started from an already-culled ~550-line Δ≤10 mask.
Restarting from the full gradient parent recovers a larger on-band pool after
rezero (738), then the RMS/√N optimizer selects the statistically strongest
subset for the aggregate mean.
