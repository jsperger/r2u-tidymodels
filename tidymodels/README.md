# R2U Tidymodels Prebuilt Image

A "batteries included" container image with a comprehensive set of pre-installed packages for R
`tidymodels` development and machine learning workflows, based on `rocker/r2u`.

> ⚠️ **PRE-ALPHA WARNING**: This image is in pre-alpha development stage. Breaking changes may occur
> without notice. Not recommended for production use or critical workflows.

Developed for my personal use and may not match your use case.

Built for Ubuntu 24.04 (`noble`) and 26.04 (`resolute`), on `amd64` and `arm64`. The suite is
selected at build time with `--build-arg R2U_SUITE=noble|resolute`.

R packages come from r2u as prebuilt Ubuntu binaries; `install.packages()` is routed through `bspm`,
which resolves system dependencies via apt.
