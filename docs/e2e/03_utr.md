# UTR

Put the same Doppler-shifted spectrum through a noiseless up-the-ramp
exposure, apply pixel calibrations in Stages 0-2, and recover the RV with
`StellarRVPrimitive`.

The template, fiber, mask, and inject grid are the same as the native L2
test. Poisson noise is off. The ramp has 10 reads.

## Setup

The ramp is built from the native rate map of the shifted template. Pixel
calibrations are simulated, not taken from KOA masters. The maps are DQ,
saturation, gain, read noise, bias, dark, and detector flat.

Stage 0 initializes DQ, checks saturation, fits the ramp (`jwst_likely`),
subtracts the dark, and applies the gain and detector flat. Bias subtraction
and the nonlinearity correction are left off, which is the default for this
simulation. Jump detection is off.

Stage 1 box-extracts with the same traces, wavelength solution, and simulated
fiber flat as the rate-map test (`fiberflat_mode` = `sim_hdf`). Stage 2
blaze-corrects. `StellarRVPrimitive` then runs with the same arguments as the
native L2 test.

## Reduction

1. Paint the shifted template and build the ramp plus pixel calibrations.
2. Run Stage 0 to an L0 rate image.
3. Box-extract and blaze-correct, as on the rate-map page.
4. Run `StellarRVPrimitive`.

The per-trial working directory is deleted after the RV is recorded. Shared
Stage-1 masters are kept.

## StellarRV arguments

Same as the [native L2](01_native_l2.md) page.

## Recovery

Bias is the fiber-average RV minus the injected RV. The sweep is
`run_step_grid` with `recover_rv_from_truth_ramp` and
`fiberflat_mode="sim_hdf"`.

## Code

| Piece | Location in `HISPEC_DRP` |
|-------|--------------------------|
| Ramp, Stage 0, then the rate-map reduction | `hispecdrp/simulations/ideal_spectrum_rv.py` (`recover_rv_from_ramp`) |
| Call used by the three-path runner | `hispecdrp/simulations/rv_step_diagnostics.py` (`recover_rv_from_truth_ramp`) |
| Ramp and pixel calibrations | `hispecdrp/simulations/simulate_up_the_ramp.py` |
