## Emacs, make this -*- mode: sh; -*-

FROM rocker/r2u:noble

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
        libgfortran \
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

## Install core tidymodels ecosystem and dependencies
RUN install.r \
    ## Core tidymodels dependencies (ensure minimum versions)
    broom \
    cli \
    conflicted \
    dials \
    dplyr \
    ggplot2 \
    hardhat \
    infer \
    modeldata \
    parsnip \
    purrr \
    recipes \
    rlang \
    rsample \
    rstudioapi \
    tibble \
    tidyr \
    tune \
    workflows \
    yardstick \
    ## Core tidymodels meta-package
    tidymodels \
    ## Extended tidymodels ecosystem
    applicable \
    butcher\
    embed \
    finetune \
    probably \
    stacks \
    textrecipes \
    themis \
    tidyposterior \
    usemodels \
    workflowsets \
    ## ML backends
    xgboost \
    lightgbm \
    randomForest \
    ranger \
    glmnet \
    kknn \
    kernlab \
    e1071 \
    ## Additional modeling frameworks
    caret \
    ## Data manipulation beyond tidyverse
    data.table \
    dtplyr \
    ## Enhanced visualization
    plotly \
    patchwork \
    cowplot \
    gganimate \
    ggforce \
    ## Development tools
    devtools \
    roxygen2 \
    testthat \
    usethis \
    pkgdown \
    ## Pipeline and workflow
    targets \
    tarchetypes \
    ## Model interpretation
    DALEX \
    iml \
    lime \
    vip \
    ## Time series (common in ML workflows)
    forecast \
    tsibble \
    fable \
    ## Database connectivity
    DBI \
    RSQLite \
    RPostgres \
    RMariaDB \
    odbc \
    ## API and web tools
    httr2 \
    jsonlite \
    rvest \
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
RUN R -e "install.packages(c('h2o'), repos='https://cran.rstudio.com/')"

## Configure R environment
RUN echo "options(repos = c(CRAN = 'https://cran.rstudio.com/'))" >> /usr/local/lib/R/etc/Rprofile.site \
    && echo "options(download.file.method = 'libcurl')" >> /usr/local/lib/R/etc/Rprofile.site \
    && echo "options(Ncpus = parallel::detectCores())" >> /usr/local/lib/R/etc/Rprofile.site

## Create a workspace directory
RUN mkdir -p /workspace
WORKDIR /workspace

## Copy example scripts and documentation
COPY README.md /workspace/

## Set default command
CMD ["R"]
