# Rate map

Paint the Doppler-shifted BT-Settl spectrum onto a native rate map, build a
fiber flat, box-extract, blaze-correct, and recover the RV with
`StellarRVPrimitive`.

The template, fiber, mask, and inject grid are the same as the native L2
test.

## Setup

The science image is a native rate map (`prf_convolution`) of the shifted
template. Stage-1 traces and the fiber flat come from the simulated fiber-flat
calibration (`fiberflat_mode` = `sim_hdf`). The wavelength solution is the
geometric HDF WAVE, unless an LFC WAVE is passed in.

The helper's default fiber flat is ones, so blaze correction does not reshape
the continuum. This page uses the simulated fiber flat.

## Reduction

1. Paint the shifted template to a native rate map.
2. Build Stage-1 calibrations: traces, wavelength solution, and the simulated fiber flat.
3. Wrap the rate map as an L0. There is no ramp and no Stage 0.
4. Box-extract (aperture 8 pixels).
5. Blaze-correct with the fiber flat.
6. Run `StellarRVPrimitive` with the same arguments as the native L2 test.

## StellarRV arguments

Same as the [native L2](01_native_l2.md) page. The search grid is centered on
the injected RV, and every finite line is kept.

## Recovery

Bias is the fiber-average RV minus the injected RV. The sweep is
`run_step_grid` with `recover_rv_from_truth_rate_map` and
`fiberflat_mode="sim_hdf"`.

## Code

| Piece | Location in `HISPEC_DRP` |
|-------|--------------------------|
| Rate map, extraction, blaze, recovery | `hispecdrp/simulations/ideal_spectrum_rv.py` (`recover_rv_from_rate_map`) |
| Call used by the three-path runner | `hispecdrp/simulations/rv_step_diagnostics.py` (`recover_rv_from_truth_rate_map`) |
