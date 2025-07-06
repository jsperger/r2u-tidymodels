## Emacs, make this -*- mode: sh; -*-

FROM rocker/r2u:noble

## This image inherits r2u binaries and bspm configuration from rocker/r2u:noble
## r2u provides faster package installation via Ubuntu binaries
## bspm automatically handles system dependencies for R packages

LABEL org.label-schema.license="GPL-2.0" \
      org.label-schema.vcs-url="https://github.com/jsperger/r2u-tidymodels" \
      org.label-schema.vendor="R2U Tidymodels Project" \
      maintainer="jsperger <jsperger@users.noreply.github.com>"

## Set environment variables for package installation
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC
ENV R_REMOTES_NO_ERRORS_FROM_WARNINGS=true

## Install additional system dependencies for ML packages
RUN apt update \
    && apt install -y --no-install-recommends \
        ## Development tools
        git \
        vim \
        nano \
        curl \
        wget \
        libgfortran-13-dev \
        ## For h2o and java-based packages
        default-jdk \
        ## For database connectivity
        libpq-dev \
        libmariadb-dev \
        unixodbc-dev \
        ## For web scraping and APIs
        libxml2-dev \
        libcurl4-openssl-dev \
        libssl-dev \
    && rm -rf /var/lib/apt/lists/*

## Install core R packages first (these change less frequently)
RUN install.r \
    ## Core tidyverse (stable, rarely change)
    dplyr \
    ggplot2 \
    purrr \
    tibble \
    tidyr \
    ## Core tidymodels dependencies (stable base)
    broom \
    cli \
    hardhat \
    rlang \
    rsample \
    yardstick

## Install tidymodels core ecosystem
RUN install.r \
    ## Core tidymodels components
    conflicted \
    dials \
    infer \
    modeldata \
    parsnip \
    recipes \
    rstudioapi \
    tune \
    workflows \
    ## Core tidymodels meta-package
    tidymodels

## Install extended tidymodels ecosystem
RUN install.r \
    ## Extended tidymodels packages
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

## Install ML backends and modeling frameworks
RUN install.r \
    ## Tree-based models
    xgboost \
    lightgbm \
    randomForest \
    ranger \
    ## Regularized models
    glmnet \
    ## Other ML algorithms
    kknn \
    kernlab \
    e1071 \
    ## Traditional ML framework
    caret

## Install data manipulation and visualization packages
RUN install.r \
    ## Data manipulation
    data.table \
    dtplyr \
    ## Enhanced visualization
    plotly \
    patchwork \
    cowplot \
    gganimate \
    ggforce

## Install development and workflow tools
RUN install.r \
    ## Development tools
    devtools \
    roxygen2 \
    testthat \
    usethis \
    pkgdown \
    ## Pipeline and workflow
    targets \
    tarchetypes

## Install model interpretation and analysis tools
RUN install.r \
    ## Model interpretation
    DALEX \
    iml \
    lime \
    vip \
    ## Time series (common in ML workflows)
    forecast \
    tsibble \
    fable

## Install database and connectivity packages
RUN install.r \
    ## Database connectivity
    DBI \
    RSQLite \
    RPostgres \
    RMariaDB \
    odbc \
    ## API and web tools
    httr2 \
    jsonlite \
    rvest

## Install parallel processing and deployment tools
RUN install.r \
    ## Parallel processing
    foreach \
    doParallel \
    future \
    furrr \
    ## Model deployment helpers
    vetiver \
    plumber \
    ## Documentation and reporting
    rmarkdown \
    quarto \
    gt \
    gtsummary

## Install packages that may require compilation (do these last)
RUN R -e "install.packages(c('h2o'), repos='https://cran.rstudio.com/')" \
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds

## Configure R environment and clean up
RUN echo "options(repos = c(CRAN = 'https://cran.rstudio.com/'))" >> /usr/local/lib/R/etc/Rprofile.site \
    && echo "options(download.file.method = 'libcurl')" >> /usr/local/lib/R/etc/Rprofile.site \
    && echo "options(Ncpus = parallel::detectCores())" >> /usr/local/lib/R/etc/Rprofile.site \
    ## Strip binary installed libraries from RSPM to reduce image size
    && strip /usr/local/lib/R/site-library/*/libs/*.so 2>/dev/null || true \
    ## Clean up any remaining package build artifacts
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds /var/lib/apt/lists/* \
    ## Create workspace directory
    && mkdir -p /workspace
WORKDIR /workspace

## Copy example scripts and documentation
COPY README.md /workspace/

## Set default command
CMD ["R"]
