# aabor/rbase
# configured for automatic build
FROM rocker/r-ver:3.5.2

LABEL maintainer="A. Borochkin"

# installation utilities
RUN  apt-get update && apt-get install -y \
  apt-utils wget zip unzip make cron nano vim \
  build-essential \
  && apt-get clean
# ssl 
RUN  apt-get update && apt-get install -y \
  libssl-dev \
  libcurl4-openssl-dev \
  && apt-get clean

# ssh 
RUN  apt-get update && apt-get install -y \
  openssh-server \
  xclip \
  gnupg2 \
  && apt-get clean

## install Fonts
RUN apt-get update && apt-get install -y libfreetype6-dev \
  libgtk2.0-dev \
  libxt-dev \
  libcairo2-dev \
  && apt-get clean

RUN apt-get update && apt-get install -y \
  libxml2-dev \
  libapparmor-dev

RUN install2.r --error \
  sys \
  #askpass \
  openssl \
  Rcpp curl httr xml2 rvest \
  && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \
  glue stringi stringr selectr magrittr \
  tidyverse \
  && rm -rf /tmp/downloaded_packages/

# 'TrueType', 'OpenType', Type 1, web fonts, etc.
# in R graphs
RUN install2.r --error \
  showtext \
  && rm -rf /tmp/downloaded_packages/

#Additional ggplot packages
RUN install2.r --error \
    # Based Publication Ready Plots
    ggpubr \
    # Provides text and label geoms for 'ggplot2' that help to avoid overlapping text labels. Labels repel away from each other and away from the data points
    ggrepel \
    # Network Analysis and Visualization
    igraph \
    && rm -rf /tmp/downloaded_packages/

# install poppler, some dependencies of other R packages
RUN apt-get update && apt-get install -y \
    ## poppler to install pdftools to work with .pdf files
    libpoppler-cpp-dev \
    ## system dependency of hunspell (devtools)
    libhunspell-dev \
    ## system dependency of hadley/pkgdown
    libmagick++-dev \
    ## (GSL math library dependencies)
    # for topicmodels on which depends textmineR
    gsl-bin \
    libgsl0-dev \
    librdf0-dev \
    && apt-get clean

# Dependencies for Rmpfr, gmp
RUN apt-get update && apt-get install -y \
    libgmp-dev \
    libmpfr-dev \
    && apt-get clean

RUN install2.r --error \ 
  # Multiple Precision Arithmetic (big integers and rationals, prime number tests, matrix computation), "arithmetic without limitations" using the C library GMP (GNU Multiple Precision Arithmetic)
  gmp \
  # Multiple Precision Floating-Point Reliable
  Rmpfr \
  # PMCMRplus: Calculate Pairwise Multiple Comparisons of Mean Rank Sums Extended
  PMCMRplus \
  && rm -rf /tmp/downloaded_packages/


## Install R packages for C++
RUN install2.r --error \
  #Run 'R CMD check' from 'R' programmatically, and capture the results of the individual checks
  rcmdcheck \
  bindr \
  inline \
  rbenchmark \
  RcppArmadillo \
  RUnit \
  highlight \
  && rm -rf /tmp/downloaded_packages/

# For developers
RUN install2.r --error \
  # to install from github
  devtools \
  # R testing environment
  testthis \
  && rm -rf /tmp/downloaded_packages/

# System utilities
RUN install2.r --error \
  #A cross-platform interface to file system operations, built on top of the 'libuv' C library
  fs \
  #  Miscellaneous functions commonly used in other packages maintained by 'Yihui Xie'
  xfun \
  # General Network (HTTP/FTP/...) Client Interface for R
  RCurl \
  && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \ 
    #Functions for latent class analysis, short time Fourier transform, fuzzy clustering, support vector machines,
    #shortest path computation, bagged clustering, naive Bayes classifier
    e1071 \
    # Gaussian Mixture Modelling for Model-Based Clustering, Classification, and Density Estimation
    mclust \
    # Companion to Applied Regression
    car \
    # A range of axis labeling algorithms
    labeling \
    # Forecasting Functions for Time Series and Linear Models
    forecast \
    # Linear Mixed-Effects Models using 'Eigen' and S4
    lme4 \
    # Classification and Regression Training
    caret \
    # Breiman and Cutler's Random Forests for Classification and Regression
    randomForest \
    # Solve optimization problems using an R interface to NLopt. NLopt is a free/open-source library for nonlinear
    # optimization.
    nloptr \
    # A collection of dimensionality reduction techniques from R packages and a common interface for calling the methods
    dimRed \
    #  A collection of tests, data sets, and examples for diagnostic checking in linear regression models. 
    lmtest \
    # core survival analysis routines, including definition of Surv objects, Kaplan-Meier and Aalen-Johansen
    # (multi-state) curves, Cox models, and parametric accelerated failure time models.
    survival \
    # Tools for Descriptive Statistics first descriptive tasks in data analysis, consisting of calculating descriptive
    # statistics, drawing graphical summaries and reporting the results %overlaps% determines if two date ranges overlap
    DescTools \
    && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \
    # A Tool Kit for Working with Time Series in R
    timetk \
    # Bringing financial analysis to the 'tidyverse'. The 'tidyquant' package provides a convenient wrapper to various
    # 'xts', 'zoo', 'quantmod', 'TTR' and 'PerformanceAnalytics' package functions and returns the objects in the tidy
    # 'tibble' format. The main advantage is being able to use quantitative functions with the 'tidyverse' functions
    # including 'purrr', 'dplyr', 'tidyr', 'ggplot2', 'lubridate', etc.
    tidyquant \
    && rm -rf /tmp/downloaded_packages/

# connection to other applications: Excel, Acrobat, ...
RUN install2.r --error \ 
    # Text Extraction, Rendering and Converting of PDF Documents. Utilities based on 'libpoppler' for extracting text,
    # fonts, attachments and metadata from a PDF file. Also supports high quality rendering of PDF documents info PNG,
    # JPEG, TIFF format, or into raw bitmap vectors
    pdftools \
    # Simplifies the creation of Excel .xlsx files by providing a high level interface to writing, styling and editing
    # worksheets. Through the use of 'Rcpp', read/write times are comparable to the 'xlsx' and 'XLConnect' packages with
    # the added benefit of removing the dependency on Java.
    openxlsx \
    && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \ 
    # easy and simple way to read, write and display bitmap images stored in the JPEG format
    png \
    # easy and simple way to read, write and display bitmap images stored in the PNG format
    jpeg \
    # Retrieve data from RSS/Atom feeds
    feedeR \
    && rm -rf /tmp/downloaded_packages/

# RESTful API
RUN install2.r --error \ 
    # An interface to the Gmail RESTful API. Allows access to your Gmail messages, threads, drafts and labels.
    gmailr \
    #Gives the ability to automatically generate and serve an HTTP API from R functions using the annotations in the R
    #documentation around your functions.
    plumber \
    # Pure R implementation of the ubiquitous log4j package. It offers hierarchic loggers, multiple handlers per logger,
    # level based filtering, space handling in messages and custom formatting.
    logging \
    && rm -rf /tmp/downloaded_packages/

# create a non-root user named docker, give them the password "docker" put them in the sudo group
RUN useradd -d /home/docker -m -s /bin/bash docker && echo "docker:docker" | chpasswd && adduser docker sudo

# start working in the "docker" home directory
WORKDIR /home/docker

# Make the files owned by docker
RUN chown -R docker:docker /home/docker

# Switch to your new user in the docker image
USER docker
