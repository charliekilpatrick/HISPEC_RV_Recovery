# HISPEC RV Recovery

Static documentation of radial-velocity (RV) recovery tests performed with HISPEC_DRP.

## Contents

- [Overview](docs/00_overview.md)

### Basic

Limited tests. A spectrum is painted directly onto a wavelength grid and passed to `StellarRVPrimitive`.

- [Single Gaussian Line](docs/basic/01_gaussian_line.md)
- [Single Template Line](docs/basic/02_template_line.md)
- [Several Lines on One Order](docs/basic/03_template_order_lines.md)

### End to end

The same injected spectrum, recovered after each additional stage of the reduction.

- [Native L2](docs/e2e/01_native_l2.md)
- [Rate map](docs/e2e/02_rate_map.md)
- [UTR](docs/e2e/03_utr.md)
