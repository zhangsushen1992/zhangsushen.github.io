## Mini-project: A Pair trading strategy using Johansen Test

Johansen test are statistical test for cointegration of two time series. Cointegration means the linear combination of the two time series generates a stationary series. Johansen test is commonly seen in mean reversion trading strategies. Here we present a simple trading strategy using Johansen test.

### 1. Reading stock data
We use **tushare** to read the data from Chinese equities. The stocks we chose are '600028' (Sinopec) and ‘601857’ (PetrolChina). The time is from 01/05/2018 to 01/05/2019.
```
import tushare

df_A = ts.get_hist_data('600028',start = '2018-05-01', end = '2019-05-01')
df_B = ts.get_hist_data('601857',start = '2018-05-01', end = '2019-05-01')
print(df_A.head())
print(df_B.head())
```

### 2. Johansen test function
The Johansen test is packaged in the module Johansen test. Here we import the module to calculate the hedge ratio.
