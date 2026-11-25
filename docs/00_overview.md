# Overview

## Purpose

Document HISPEC ideal-spectrum RV recovery experiments: how they were run in
`HISPEC_DRP`, what masks and inject grids were used, and the numerical / graphical
results. No pipeline execution is required to read this repo.

## Source code (sibling repo)

| Item | Location in `HISPEC_DRP` |
|------|---------------------------|
| Multi-line Δ notebook | `examples/notebooks/multi_line_centroid_ccf.ipynb` |
| RMS/√N optimizer | `examples/scripts/scratch/optimize_mask_rms_over_sqrt_n.py` |
| Iterative ccfopt rezero | `hispecdrp/utils/line_mask_utils.py` → `iteratively_rezero_ccfopt_line_mask` |
| Gradient parent mask | `hispecdrp/data/masks/lte040.0-4.5-0.0a+0.0.BT-Settl.spec.7.delta.mas` |
| Ideal L2 paint | `hispecdrp/simulations/spectra.py` → `build_l2` |

Local scratch outputs typically live under `HISPEC_DRP/test_data/` (gitignored) or
`~/test_data/…`. Figures and numbers here are **copied snapshots** of those runs.

## Template / instrument defaults for these pages

- Template: BT-Settl `lte040.0-4.5-0.0a+0.0.BT-Settl.spec.7`
- Spectrograph: BSPEC (orders ~110–148)
- Fiber: 1
- Paint oversampling: `n_fine=512`
- Inject grid (unless noted): \(0,1,\ldots,10\) km/s

## Metric definitions

For line \(i\) at injected bulk velocity \(v\):

\[
\mathrm{bias}_i(v) = \mathrm{RV}_i(v) - v,\qquad
\Delta_i(v) = \mathrm{bias}_i(v) - \mathrm{bias}_i(0)
\]

Aggregate line scatter and mean-RV statistical scale:

\[
\mathrm{RMS} = \sqrt{\langle \Delta^2 \rangle},\qquad
\frac{\mathrm{RMS}}{\sqrt{N}}
\]

where \(N\) is the number of lines common (finite) across the inject grid.
