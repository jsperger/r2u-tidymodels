# R2U Tidymodels Prebuilt Image

A "batteries included" container image with a comprehensive set of pre-installed packages for R
`tidymodels` development and machine learning workflows. Built on top of
[`rocker/r2u`](https://github.com/rocker-org/r2u), which this repository forks.

> ⚠️ **PRE-ALPHA WARNING**: This image is in pre-alpha development stage. Breaking changes may occur
> without notice. Not recommended for production use or critical workflows.

Developed for my personal use and may not match your use case.

### Supported releases

Images are built for Ubuntu 24.04 (`noble`) and 26.04 (`resolute`) only, each for both `amd64` and
`arm64`.

| Release | Suite      | Tags                                                   |
| ------- | ---------- | ------------------------------------------------------ |
| 24.04   | `noble`    | `jsperger/r-r2u-tidymodels:noble`, `:24.04`, `:latest`  |
| 26.04   | `resolute` | `jsperger/r-r2u-tidymodels:resolute`, `:26.04`          |

Older releases are deliberately unsupported. r2u publishes `arm64` binaries for `noble` and
`resolute` but not for `focal` (20.04) or `jammy` (22.04) — see the
[r2u FAQ on architectures](https://eddelbuettel.github.io/r2u/vignettes/FAQ/#what-about-other-architectures-besides-x86_64).
Since every R package in this image arrives from r2u as an Ubuntu binary, a release without `arm64`
r2u coverage cannot produce an `arm64` image at all.

### Usage

```sh
docker run --rm -it -v "$(pwd)":/workspace jsperger/r-r2u-tidymodels:latest R
```

Package installation inside the container goes through
[`bspm`](https://cran.r-project.org/package=bspm), so `install.packages("somepkg")` fetches a
prebuilt Ubuntu binary and pulls in its system dependencies automatically.

### Building locally

The image is a single build context parameterised by the r2u suite:

```sh
# 24.04
docker build --build-arg R2U_SUITE=noble    -t r-r2u-tidymodels:noble    tidymodels
# 26.04
docker build --build-arg R2U_SUITE=resolute -t r-r2u-tidymodels:resolute tidymodels
```

### Upstream contexts

`noble/`, `resolute/`, and `noble_ci/` are carried unchanged from `rocker-org/r2u` so that merges
from upstream stay conflict-free. They build the `rocker/r2u:*` base images and are **not** built by
this repository's workflow.

### Background

Please see [motivation](motivation.md) for how r2u fits in with other Rocker containers.
