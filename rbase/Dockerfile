# aabor/rbase
# configured for automatic build
FROM rocker/r-ver:3.5.1

LABEL maintainer="A. Borochkin"

# installation utilities
RUN  apt-get update \
  && apt-get install -y --no-install-recommends \
    wget cron nano vim \
    build-essential \
    file \
    git \
    libapparmor1 \
    libcurl4-openssl-dev \
    libedit2 \
    libssl-dev \
    lsb-release \
    libxml2-dev \
    libsqlite3-dev \
    libmariadbd-dev \
    libmariadb-client-lgpl-dev \
    libpq-dev \
    libssh2-1-dev \
    openssh-server \
    xclip \
    unixodbc-dev \
    psmisc \
    procps \
    python-setuptools \
    sudo \
    gnupg2 \
    libfreetype6-dev \
    libgtk2.0-dev \
    libxt-dev \
    libcairo2-dev \
  && apt-get clean

# 'TrueType', 'OpenType', Type 1, web fonts, etc.
# in R graphs
RUN install2.r --error \
  showtext \
  #R string manipulation functions that account for the effects of ANSI text formatting control sequences
  fansi \
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

# tidyverse
RUN install2.r --error \
    --deps TRUE \
    tidyverse \
    dplyr \
    devtools \
    formatR \
    remotes \
    selectr \
    caTools \
    BiocManager

# for rvest
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  && wget -O libssl1.0.0.deb http://ftp.debian.org/debian/pool/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u8_amd64.deb \
  && dpkg -i libssl1.0.0.deb \
    && rm libssl1.0.0.deb
    
# web scrapping
RUN install2.r --error \ 
  curl \
  jsonlite \
  mime \
  openssl \
  httr \
  xml2 \
  # Wrappers around the 'xml2' and 'httr' packages to make it easy to download, then manipulate, HTML and XML.
  rvest \
  # An interface to the Gmail RESTful API. Allows access to your Gmail messages, threads, drafts and labels.
  gmailr \
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


# System utilities
RUN install2.r --error \
  #A cross-platform interface to file system operations, built on top of the 'libuv' C library
  fs \
  #  Miscellaneous functions commonly used in other packages maintained by 'Yihui Xie'
  xfun \
  # General Network (HTTP/FTP/...) Client Interface for R
  RCurl \
  && rm -rf /tmp/downloaded_packages/

# Setup JAVA_HOME
ENV JAVA_HOME="/usr/lib/jvm/default-jvm"

# Install Oracle JDK (Java SE Development Kit) 8u192 with Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files for JDK/JRE 8
RUN JAVA_VERSION=8 && \
    JAVA_UPDATE=192 && \
    JAVA_BUILD=12 && \
    JAVA_PATH=750e1c8617c5452694857ad95c3ee230 && \
    JAVA_SHA256_SUM=6d34ae147fc5564c07b913b467de1411c795e290356538f22502f28b76a323c2 && \
    JCE_SHA256_SUM=f3020a3922efd6626c2fff45695d527f34a8020e938a49292561f18ad1320b59 && \ 
    apt-get update && \
    cd "/tmp" && \
    wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/${JAVA_PATH}/jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
    echo "${JAVA_SHA256_SUM}" "jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" | sha256sum -c - && \
    wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" "http://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION}/jce_policy-${JAVA_VERSION}.zip" && \
    echo "${JCE_SHA256_SUM}" "jce_policy-${JAVA_VERSION}.zip" | sha256sum -c - && \
    tar -xzf "jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
    mkdir -p "/usr/lib/jvm" && \
    mv "/tmp/jdk1.${JAVA_VERSION}.0_${JAVA_UPDATE}" "/usr/lib/jvm/java-${JAVA_VERSION}-oracle" && \
    ln -s "java-${JAVA_VERSION}-oracle" "$JAVA_HOME" && \
    ln -s "$JAVA_HOME/bin/"* "/usr/bin/" && \
    unzip -jo -d "$JAVA_HOME/jre/lib/security" "jce_policy-${JAVA_VERSION}.zip" && \    
    rm -rf "$JAVA_HOME/"*src.zip \
           "$JAVA_HOME/lib/missioncontrol" \
           "$JAVA_HOME/lib/visualvm" \
           "$JAVA_HOME/lib/"*javafx* \
           "$JAVA_HOME/jre/lib/plugin.jar" \
           "$JAVA_HOME/jre/lib/ext/jfxrt.jar" \
           "$JAVA_HOME/jre/bin/javaws" \
           "$JAVA_HOME/jre/lib/javaws.jar" \
           "$JAVA_HOME/jre/lib/desktop" \
           "$JAVA_HOME/jre/plugin" \
           "$JAVA_HOME/jre/lib/"deploy* \
           "$JAVA_HOME/jre/lib/"*javafx* \
           "$JAVA_HOME/jre/lib/"*jfx* \
           "$JAVA_HOME/jre/lib/amd64/libdecora_sse.so" \
           "$JAVA_HOME/jre/lib/amd64/"libprism_*.so \
           "$JAVA_HOME/jre/lib/amd64/libfxplugins.so" \
           "$JAVA_HOME/jre/lib/amd64/libglass.so" \
           "$JAVA_HOME/jre/lib/amd64/libgstreamer-lite.so" \
           "$JAVA_HOME/jre/lib/amd64/"libjavafx*.so \
           "$JAVA_HOME/jre/lib/amd64/"libjfx*.so \
           "$JAVA_HOME/jre/bin/jjs" \
           "$JAVA_HOME/jre/bin/keytool" \
           "$JAVA_HOME/jre/bin/orbd" \
           "$JAVA_HOME/jre/bin/pack200" \
           "$JAVA_HOME/jre/bin/policytool" \
           "$JAVA_HOME/jre/bin/rmid" \
           "$JAVA_HOME/jre/bin/rmiregistry" \
           "$JAVA_HOME/jre/bin/servertool" \
           "$JAVA_HOME/jre/bin/tnameserv" \
           "$JAVA_HOME/jre/bin/unpack200" \
           "$JAVA_HOME/jre/lib/ext/nashorn.jar" \
           "$JAVA_HOME/jre/lib/jfr.jar" \
           "$JAVA_HOME/jre/lib/jfr" \
           "$JAVA_HOME/jre/lib/oblique-fonts" \           
           "$JAVA_HOME/README.html" \
           "$JAVA_HOME/THIRDPARTYLICENSEREADME-JAVAFX.txt" \
           "$JAVA_HOME/THIRDPARTYLICENSEREADME.txt" \            
           "$JAVA_HOME/jre/README" \
           "$JAVA_HOME/jre/THIRDPARTYLICENSEREADME-JAVAFX.txt" \
           "$JAVA_HOME/jre/THIRDPARTYLICENSEREADME.txt" \
           "$JAVA_HOME/jre/Welcome.html" \
           "$JAVA_HOME/jre/lib/security/README.txt" && \           
    apt-get -y clean && \
    rm -rf "/tmp/"* \
           "/var/cache/apt" \
           "/usr/share/man" \
           "/usr/share/doc" \
           "/usr/share/doc-base" \
           "/usr/share/info/*" \
  && R CMD javareconf
## make sure Java can be found in rApache and other daemons not looking in R ldpaths 
RUN echo "/usr/lib/jvm/java-8-oracle/jre/lib/amd64/server/" > /etc/ld.so.conf.d/rJava.conf 
RUN /sbin/ldconfig

## Install rJava package 
# install rJava dependencies
RUN apt-get update && apt-get install -y \
    # for Java
    libicu-dev \ 
    libbz2-dev \
    liblzma-dev \
    libpcre3-dev \
    && apt-get clean

# install rJava package
RUN install2.r --error rJava \ 
&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds

RUN install2.r --error \ 
    #Functions for latent class analysis, short time Fourier transform, fuzzy clustering, support vector machines, shortest path computation, bagged clustering, naive Bayes classifier
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
    # Solve optimization problems using an R interface to NLopt. NLopt is a free/open-source library for nonlinear optimization.
    nloptr \
    # A collection of dimensionality reduction techniques from R packages and a common interface for calling the methods
    dimRed \
    #  A collection of tests, data sets, and examples for diagnostic checking in linear regression models. 
    lmtest \
    # core survival analysis routines, including definition of Surv objects, Kaplan-Meier and Aalen-Johansen (multi-state) curves, Cox models, and parametric accelerated failure time models.
    survival \
    && rm -rf /tmp/downloaded_packages/

# connection to other applications: Excel, Acrobat, ...
RUN install2.r --error \ 
  # Text Extraction, Rendering and Converting of PDF Documents. Utilities based on 'libpoppler' for extracting text, fonts, attachments and metadata from a PDF file. Also supports high quality rendering of PDF documents info PNG, JPEG, TIFF format, or into raw bitmap vectors
  pdftools \
  # R connection to Excel, Java dependent
  XLConnect \
  # read/write/format Excel 2007 and Excel 97/2000/XP/2003 file formats
  xlsx \
  # Simplifies the creation of Excel .xlsx files by providing a high level interface to writing, styling and editing worksheets. Through the use of 'Rcpp', read/write times are comparable to the 'xlsx' and 'XLConnect' packages with the added benefit of removing the dependency on Java.
  openxlsx \
  && rm -rf /tmp/downloaded_packages/

# Graphics Java script libraries
RUN install2.r --error \ 
  # A graphical display of a correlation matrix or general matrix. It also contains some algorithms to do matrix reordering. In addition, corrplot is good at details, including choosing color, text labels, color labels, layout, etc.
  corrplot \
  && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \ 
    # easy and simple way to read, write and display bitmap images stored in the JPEG format
    png \
    # easy and simple way to read, write and display bitmap images stored in the PNG format
    jpeg \
    # Retrieve data from RSS/Atom feeds
    feedeR \
    && rm -rf /tmp/downloaded_packages/

# selenium
RUN sudo installGithub.r \
  johndharrison/binman \
  johndharrison/wdman \ 
  ropensci/RSelenium \
  && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \ 
    #Gives the ability to automatically generate and serve an HTTP API from R functions using the annotations in the R documentation around your functions.
    plumber \
    # Pure R implementation of the ubiquitous log4j package. It offers hierarchic loggers, multiple handlers per logger, level based filtering, space handling in messages and custom formatting.
    logging \
    && rm -rf /tmp/downloaded_packages/

