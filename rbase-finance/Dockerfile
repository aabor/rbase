# aabor/rfin
FROM aabor/rbase

USER root

# finance, forcasting
RUN installGithub.r \
    # Tools for Transaction-Oriented Trading Systems Development
    braverock/blotter \
    # Specify, build, and back-test quantitative financial trading and portfolio strategies.
    braverock/quantstrat \
    && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \ 
    # Tools for Descriptive Statistics first descriptive tasks in data analysis,
    # consisting of calculating descriptive statistics, drawing graphical summaries
    # and reporting the results
    # %overlaps% determines if two date ranges overlap
    DescTools \
    # Quantitative Financial Modelling Framework
    quantmod \
    # Tools for Quantitative Risk Management
    qrmtools \
    # Various data sets (stocks, stock indices, constituent data, FX, zero-coupon bond yield curves, volatility, commodities) for Quantitative Risk Management practice
    qrmdata \
    && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \ 
    #ARFIMA, in-mean, external regressors and various GARCH flavors, with methods for fit, forecast, simulation, inference and plotting
    rugarch \
    && rm -rf /tmp/downloaded_packages/

 # QuantLib installation
RUN  apt-get update && apt-get install -y \
    # QuantLib Library aims to provide a comprehensive software framework for quantitative finance. The goal is to provide a standard open source library for quantitative analysis, modeling, trading, and risk management of financial assets    libboost-all-dev \
    r-cran-rquantlib quantlib-examples libquantlib0-dev

RUN install2.r --error \
    # A Tool Kit for Working with Time Series in R
    timetk \
    # Bringing financial analysis to the 'tidyverse'. The 'tidyquant' package provides a convenient wrapper to various 'xts', 'zoo', 'quantmod', 'TTR' and 'PerformanceAnalytics' package functions and returns the objects in the tidy 'tibble' format. The main advantage is being able to use quantitative functions with the 'tidyverse' functions including 'purrr', 'dplyr', 'tidyr', 'ggplot2', 'lubridate', etc.
    tidyquant \
    && rm -rf /tmp/downloaded_packages/

# Switch to your new user in the docker image
USER docker

    
