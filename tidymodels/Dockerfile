## Emacs, make this -*- mode: sh; -*-

## Only noble (24.04) and resolute (26.04) are supported: r2u ships arm64 binaries
## for these two suites only. See https://eddelbuettel.github.io/r2u/vignettes/FAQ/
ARG R2U_SUITE=noble
FROM rocker/r2u:${R2U_SUITE}

## This image inherits r2u binaries and bspm configuration from rocker/r2u.
## Every R package below installs as a prebuilt Ubuntu binary (r-cran-*), and bspm
## pulls in each package's system dependencies automatically. This is a single-stage
## build: a builder/final split cannot work here, because the packages land in
## /usr/lib/R/site-library as dpkg-owned debs whose shared-library dependencies are
## spread across the root filesystem and would not survive a COPY --from.

LABEL org.label-schema.license="GPL-2.0" \
      org.label-schema.vcs-url="https://github.com/jsperger/r2u-tidymodels" \
      org.label-schema.vendor="R2U Tidymodels Project" \
      maintainer="jsperger <jsperger@users.noreply.github.com>"

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC
ENV R_REMOTES_NO_ERRORS_FROM_WARNINGS=true

## Parallelism for install2.r; -1 means "use all available cores"
ARG NCPUS=-1

## Toolchain and headers, so packages *not* in r2u can still be built from source by a
## user of this image. Use unversioned `gfortran`, which depends on the matching
## libgfortran-N-dev for the release, rather than pinning a compiler generation.
RUN apt update \
    && apt install -y --no-install-recommends \
        git \
        vim \
        nano \
        curl \
        wget \
        gfortran \
        default-jdk \
        libpq-dev \
        libmariadb-dev \
        unixodbc-dev \
        libxml2-dev \
        libcurl4-openssl-dev \
        libssl-dev \
    && rm -rf /var/lib/apt/lists/*

## install2.r --error makes a missing or broken package fail the build.
## Plain install.r exits 0 even when a package cannot be installed.

## Core tidyverse and the stable tidymodels base
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    dplyr \
    ggplot2 \
    purrr \
    tibble \
    tidyr \
    broom \
    cli \
    hardhat \
    rlang \
    rsample \
    yardstick

## Core tidymodels ecosystem
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    conflicted \
    dials \
    infer \
    modeldata \
    parsnip \
    recipes \
    rstudioapi \
    tune \
    workflows \
    tidymodels

## Extended tidymodels ecosystem
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    applicable \
    butcher \
    embed \
    finetune \
    probably \
    stacks \
    textrecipes \
    themis \
    tidyposterior \
    usemodels \
    workflowsets

## ML backends and modeling frameworks
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    xgboost \
    lightgbm \
    randomForest \
    ranger \
    glmnet \
    kknn \
    kernlab \
    e1071 \
    caret \
    h2o

## Data manipulation and visualization
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    data.table \
    dtplyr \
    plotly \
    patchwork \
    cowplot \
    gganimate \
    ggforce

## Development and workflow tooling
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    devtools \
    roxygen2 \
    testthat \
    usethis \
    pkgdown \
    targets \
    tarchetypes

## Model interpretation, analysis, and time series
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    DALEX \
    iml \
    lime \
    vip \
    forecast \
    tsibble \
    fable

## Database connectivity and web APIs
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    DBI \
    RSQLite \
    RPostgres \
    RMariaDB \
    odbc \
    httr2 \
    jsonlite \
    rvest

## Parallel processing, deployment, and reporting
RUN install2.r --error --skipinstalled -n "${NCPUS}" \
    foreach \
    doParallel \
    future \
    furrr \
    vetiver \
    plumber \
    rmarkdown \
    quarto \
    gt \
    gtsummary

## Rprofile.site lives at R_HOME/etc, i.e. /usr/lib/R/etc, symlinked to /etc/R. The CRAN
## repo is already set by the base image; overriding it here would bypass r2u.
RUN echo "options(Ncpus = parallel::detectCores())" >> /etc/R/Rprofile.site \
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds /var/lib/apt/lists/* \
    && mkdir -p /workspace

WORKDIR /workspace

COPY README.md /workspace/

CMD ["R"]
