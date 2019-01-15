# aabor/rfin
FROM aabor/rbase
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

RUN install2.r --error \ 
    # QuantLib Library aims to provide a comprehensive software framework for quantitative finance. The goal is to provide a standard open source library for quantitative analysis, modeling, trading, and risk management of financial assets
    RQuantLib \
    && rm -rf /tmp/downloaded_packages/
    
