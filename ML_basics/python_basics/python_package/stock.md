## Analysis of Stock Data

First import relevant modules:
```
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
```

Load stock data:
```
stock_max, stock_min, stock_close, stock_amount = np.loadtxt('..\\SH600000.txt', delimiter='\t', skiprows=2, usecols=(2,3,4,5), unpack=True)
N=100
stock_close = stock_close[:N]
print(stock_close)
```

