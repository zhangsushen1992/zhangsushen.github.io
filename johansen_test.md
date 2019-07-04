## Mini-project: A Pair trading strategy using Johansen Test

Johansen test are statistical test for cointegration of two time series. Cointegration means the linear combination of the two time series generates a stationary series. Johansen test is commonly seen in mean reversion trading strategies. Here we present a simple trading strategy using Johansen test.

###1. Reading stock data
We use **tushare** to read the data from Chinese equities. The stocks we chose are '600028' (Sinopec) and 
