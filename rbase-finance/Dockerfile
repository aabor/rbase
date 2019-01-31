# aabor/rfin
FROM aabor/rbase

USER root

RUN install2.r --error \ 
    remotes \
    && rm -rf /tmp/downloaded_packages/

# finance, forcasting
RUN installGithub.r \
    # Tools for Transaction-Oriented Trading Systems Development
    braverock/blotter \
    # Specify, build, and back-test quantitative financial trading and portfolio strategies.
    braverock/quantstrat \
    && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \ 
    # Tools for Quantitative Risk Management
    qrmtools \
    # Various data sets (stocks, stock indices, constituent data, FX, zero-coupon bond yield curves, volatility,
    # commodities) for Quantitative Risk Management practice
    qrmdata \
    && rm -rf /tmp/downloaded_packages/

RUN install2.r --error \ 
    #ARFIMA, in-mean, external regressors and various GARCH flavors, with methods for fit, forecast, simulation,
    #inference and plotting 
    rugarch \
    && rm -rf /tmp/downloaded_packages/

 # QuantLib installation
RUN  apt-get update && apt-get install -y \
    # QuantLib Library aims to provide a comprehensive software framework for quantitative finance. The goal is to
    # provide a standard open source library for quantitative analysis, modeling, trading, and risk management of
    # financial assets    
    libboost-all-dev \
    r-cran-rquantlib quantlib-examples libquantlib0-dev

# Switch to your new user in the docker image
USER docker

    
