# Overview

## Purpose

Document HISPEC ideal-spectrum RV recovery experiments.

## Source code

| Test | Relevant code |
|------|----------------|
| Single Gaussian line | `hispecdrp/simulations/gaussian_line_rv.py` |
| Single template line | `hispecdrp/simulations/template_line_rv.py` |
| Several lines on one order | `hispecdrp/simulations/template_order_lines_rv.py` |

## Template / instrument defaults for these pages

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
