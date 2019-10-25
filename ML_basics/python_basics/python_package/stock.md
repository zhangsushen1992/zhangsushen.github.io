## Analysis of Stock Data

First import relevant modules:
```
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
import codecs
```

Load stock data, using codecs to decode the chinese characters in the txt file. 
```
myfile='/Users/sushenzhang/Documents/programming/小象/机器学习升级版/5.Package/SH600000.txt'
filecp = codecs.open(myfile, encoding = 'cp1252')
stock_max, stock_min, stock_close, stock_amount = np.loadtxt(filecp, delimiter='\t', skiprows=2, usecols=(2,3,4,5), unpack=True)
```
Choose the first 100 data points for experimentation. 
```
N=100
stock_close = stock_close[:N]
print(stock_close)
```
n is the number of days we have the moving average. For the Simple moving average, weightings are equal for different days. Use np.convolve to obtain the array with stock close price times the weight.
```
n = 10
weight = np.ones(n)
weight /= weight.sum()
stock_sma = np.convolve(stock_close, weight, mode='valid')
```
The exponential moving average is developed by exponentiate numbers between 1 and 0 with the first number having the highest weight.
```
weight = np.linspace(1, 0, n)
weight = np.exp(weight)
weight /= weight.sum()
stock_ema = np.convolve(stock_close, weight, mode='valid')
```
t is the range of moving average days. We choose days from n-1 to N to have values for moving averages, i.e. the first few days does not have enough history to calculate moving averages. We use polyfit to fit a polynomial to moving average data and evaluate at the x positions represented by array t.
```
t = np.arange(n-1, N)
poly = np.polyfit(t, stock_ema, 10)
stock_ema_hat = np.polyval(poly, t)
```
Plot the close price, the simple moving average and the exponential moving average on the graph.
```
mpl.rcParams['font.sans-serif'] = ['SimHei']
mpl.rcParams['axes.unicode_minus'] = False
plt.figure(facecolor='w')
plt.plot(np.arange(N), stock_close, 'ro-', linewidth=2, label='Original close price')
t = np.arange(n-1, N)
plt.plot(t, stock_sma, 'b-', linewidth=2, label='Simple moving average')
plt.plot(t, stock_ema, 'g-', linewidth=2, label='Exponential moving average')
plt.legend(loc='upper right')
plt.title('Close price and Moving Averages', fontsize=15)
plt.grid(True)
plt.show()
```
Plot the close price, the exponential moving average and the fitted polynomial of estimated moving average on the same graph.
```
plt.figure(figsize=(8.8, 6.6), facecolor='w')
plt.plot(np.arange(N), stock_close, 'ro-', linewidth=1, label='Original close price')
plt.plot(t, stock_ema, 'g-', linewidth=2, label='Exponential moving average')
plt.plot(t, stock_ema_hat, '-', color='#FF4040', linewidth=3, label='Estimation of exponential moving average')
plt.legend(loc='upper right')
plt.title('Moving Average Estimation', fontsize=15)
plt.grid(True)
plt.show()
```
