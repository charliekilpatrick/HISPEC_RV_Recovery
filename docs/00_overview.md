# Overview

## Purpose

Document HISPEC ideal-spectrum RV recovery experiments. The basic pages paint
a spectrum onto a wavelength grid and stop there. The end-to-end pages take
the same kind of injection through the reduction.

## Basic tests

These are limited tests. There is no detector image and no calibration
pipeline.

| Test | Relevant code |
|------|----------------|
| Single Gaussian line | `hispecdrp/simulations/gaussian_line_rv.py` |
| Single template line | `hispecdrp/simulations/template_line_rv.py` |
| Several lines on one order | `hispecdrp/simulations/template_order_lines_rv.py` |

## End-to-end tests

One BT-Settl spectrum is Doppler-shifted, then recovered on three paths.
Each path adds the next stage of the reduction and ends in `StellarRVPrimitive`.

| Test | What runs | Relevant code |
|------|-----------|----------------|
| Native L2 | Paint onto the L2 wavelength grid, then `StellarRVPrimitive` | `recover_rv_from_truth_l2` |
| Rate map | Native rate map and fiber flat, box extraction, blaze correction, then `StellarRVPrimitive` | `recover_rv_from_rate_map` |
| UTR | Noiseless ramp, pixel calibrations through Stages 0-2, then `StellarRVPrimitive` | `recover_rv_from_ramp` |

The runners live in `hispecdrp/simulations/rv_step_diagnostics.py` and
`hispecdrp/simulations/ideal_spectrum_rv.py`. The inject grid for these
pages is $0, 10, \ldots, 50$ km/s.

## Template / instrument defaults for the basic pages

- Template: BT-Settl `lte040.0-4.5-0.0a+0.0.BT-Settl.spec.7`
- Spectrograph: BSPEC (orders ~110-148)
- Fiber: 1
- Paint oversampling: `n_fine=512`
- Inject grid (unless noted): $0,1,\ldots,10$ km/s

## Metric definitions

For line $i$ at injected bulk velocity $v$:

$$\mathrm{bias}_i(v) = \mathrm{RV}_i(v) - v,\qquad \Delta_i(v) = \mathrm{bias}_i(v) - \mathrm{bias}_i(0)$$

Aggregate line scatter and mean-RV statistical scale:

$$\mathrm{RMS} = \sqrt{\langle \Delta^2 \rangle},\qquad \frac{\mathrm{RMS}}{\sqrt{N}}$$

where $N$ is the number of lines common (finite) across the inject grid.
On the end-to-end pages the reported RV is the fiber average, so $N$ is one
per trial and the bias is that average minus the injected velocity.
