# Single Gaussian line

Idealized injection of one absorption line on a single order, at infinite
signal to noise. `StellarRVPrimitive` recovers that line at every injected RV.

## Setup

Order 136 is a 4096-pixel linear wavelength grid centered at 1.080882 um
(synthetic echelle scaling, center wavelength `147/m` um). The pixel scale is
538 m/s. Continuum flux is 1. The line is a Gaussian in velocity,

$$F(v)=1-D\exp\left(-\frac{1}{2}\left(\frac{v-v_{\mathrm{inj}}}{\sigma_v}\right)^{2}\right),$$

with $D=0.6$ and $`\sigma_v=4`$ km/s, and $`v=c(\lambda/\lambda_0-1)`$.

Per-pixel flux error is $10^{-8}$ (continuum SNR $10^{8}$ per pixel). The floor
keeps the CCF weights finite. The centroid is the infinite-SNR limit.

The mask is one delta line at the rest wavelength. The injected spectrum is
kept in float64. Rounding it to the Level-2 float32 dtype adds several
hundredths of a mm/s, which would hide the residual this setup is measuring.

## StellarRV arguments

`StellarRVPrimitive` runs `ccf_line_mask` with the search grid centered on the
injected RV. Resampling onto that grid is turned off, and the Gaussian fit
leaves the continuum free.

| Argument | Value |
|----------|------:|
| `mask_ccf_resample` | false |
| `mask_ccf_fit_baseline_free` | true |
| `v_half_window` | 20000 m/s |
| `window_line_by_line` | true |
| spectral window | +/- 20 line widths |
| `ccf_input_velocity` | injected RV |

With resampling off, one delta line is read at the pixel velocities
$`v=c((\lambda/\lambda_0)(1+z_b)-1)`$. No Lanczos kernel is applied. The
uniform velocity grid remains the default for every other call
(`mask_ccf_resample` defaults to true).

## Recovery

Injected RV runs from -20 km/s to 20 km/s in steps of 1 km/s (41 trials). That
step is about two pixels, so the line center falls at different sub-pixel phases.

| Quantity | Value |
|----------|------:|
| Max absolute bias | $4.5\times 10^{-10}$ mm/s |
| RMS bias | $9.4\times 10^{-11}$ mm/s |
| Max formal RV uncertainty | $5.4\times 10^{-4}$ mm/s |

The same recovery holds when the velocity prior is shifted by +2 km/s from the
injected value: the fit returns the line center, not the prior. The unit test
requires the absolute bias to stay under $10^{-8}$ mm/s.

## Remaining residual

The pixels already hold the injected Gaussian, and the centroid reads those
pixels directly. The largest leftover on this grid is a few times
$10^{-13}$ m/s, which is the float64 scatter of the fit.

Leaving resampling on does not reach that. Lanczos-8 replaces the Gaussian
with an approximation, and the default fit holds the continuum at a
sigma-clipped median. At a quarter pixel that median is high by 0.012, and the
iteration stops 0.013 mm/s from the injected RV. The curve below is that
chi-squared, in units of the stopping tolerance. Both centers sit on the flat
bottom. `mask_ccf_fit_baseline_free` selects the existing free-continuum
LevMar fit; it does not change the solver. Uniform weights and a tighter stop
are not separate parameters, and they are not needed once the samples are the
pixels and the continuum is free.

![Chi-squared of the resampled Gaussian fit](../assets/plots/gaussian_line_fit_valley.png)

Machine-readable summary:
[`assets/tables/gaussian_line_rv_summary.json`](../assets/tables/gaussian_line_rv_summary.json).

## Line profile

Simulated pixels at RV = 0 and the free-continuum Gaussian fit on those pixels.
The largest flux residual is $1.1\times 10^{-16}$.

![Simulated line and Gaussian fit](../assets/plots/gaussian_line_profile_fit.png)

## Recovered RV

Recovered RV versus injected RV. The top panel sits on the one-to-one line.
Residuals away from zero in the lower panel are one last place of the fitted
mean, and they fall within a few km/s of zero. The largest residual on this
grid is $4.5\times 10^{-10}$ mm/s.

![Gaussian line RV recovery](../assets/plots/gaussian_line_rv_recovery.png)

## Code

| Piece | Location in `HISPEC_DRP` |
|-------|--------------------------|
| Injection and recovery | `hispecdrp/simulations/gaussian_line_rv.py` |
| Unit test | `tests/simulations/test_gaussian_line_rv.py` |
