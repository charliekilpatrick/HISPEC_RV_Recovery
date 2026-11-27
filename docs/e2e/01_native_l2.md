# Native L2

Inject a radial velocity into the BT-Settl lte040 template, paint the shifted
spectrum onto the native BSPEC L2 wavelength grid, and recover it with
`StellarRVPrimitive`. This path does not build a detector image.

## Setup

The template is BT-Settl `lte040.0-4.5-0.0a+0.0.BT-Settl.spec.7` (4000 K).
The spectrograph is BSPEC, fiber 1. `build_l2` Doppler-shifts the template
and paints it onto the detector wavelength grid.

The mask is the packaged order-136 fine-paint ccfopt mask,
`lte040.0-4.5-0.0a+0.0.BT-Settl.spec.7.order136.iso14.nfine512.ccfopt.delta.mas`.
`StellarRVPrimitive` returns the fiber-average RV of those lines. Every finite
line is kept (`outlier_sigma` = $10^{30}$).

Injected RV runs from 0 km/s to 50 km/s in steps of 10 km/s.

## StellarRV arguments

`StellarRVPrimitive` runs `ccf_line_mask`. The search grid is centered on the
injected RV.

| Argument | Value |
|----------|------:|
| `v_step_coarse` | 10 m/s |
| `v_half_window` | 20000 m/s |
| `mask_ccf_pixel_interp_order` | 3 (Lanczos-3) |
| `window_line_by_line` | true |
| `window_size_um` | $5.65\times 10^{-5}$ um |
| `outlier_sigma` | $10^{30}$ |
| `ccf_input_velocity` | injected RV |

## Recovery

Bias is the fiber-average RV minus the injected RV. The sweep is
`run_step_grid` with `recover_rv_from_truth_l2`.

## Code

| Piece | Location in `HISPEC_DRP` |
|-------|--------------------------|
| L2 paint and recovery | `hispecdrp/simulations/rv_step_diagnostics.py` (`recover_rv_from_truth_l2`) |
| Paint | `hispecdrp/simulations/spectra.py` (`build_l2`) |
| `StellarRVPrimitive` call | `hispecdrp/simulations/spectra.py` (`recover_stellar_rv`) |
