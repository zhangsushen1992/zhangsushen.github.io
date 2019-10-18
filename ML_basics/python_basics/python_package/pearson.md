## Plotting Pearson Correlation Coefficient
Import packages:
```
import numpy as np
from scipy import stats
import matplotlib as mpl
import matplotlib.pyplot as plt
import warnings
mpl.rcParams['axes.unicode_minus']=False
```
Define function to calculate Pearson coefficient:
```
def calc_pearson(x, y):
    std1 = np.std(x)
    std2 = np.std(y)
    cov = np.cov(x, y, bias=True)[0,1]
    return cov / (std1*std2)
```
A function to prove the calculated pearson is correct:
```
def intro():
    N = 10
    x = np.random.rand(N)
    y = 2 * x + np.random.randn(N) * 0.1
    print('system calculates:', stats.pearsonr(x,y)[0])
    print('hand calculates:', calc_pearson(x,y))
```
Rotate a function:
```
def rotate(x, y, theta=45):
    data = np.vstack((x,y))
    mu = np.mean(data, axis=1)
    mu = mu.reshape(-1,1)
    theta *= (np.pi/180) # convert to radian
    c = np.cos(theta)
    s = np.sin(theta)
    m = np.array(((c, -s),(s, c)))
    return m.dot(data) + mu
```
Draw the graph of rotating functions and output the Pearson coefficient:
```
def pearson(x, y, tip):
    clrs = list('rgbmycrgbmycrgbmycrgbmyc')
    plt.figure(figsize=(10,8), facecolor='w')
    for i, theta in enumerate(np.linspace(0, 90, 6)):
        xr, yr = rotate(x, y, theta)
        p = stats.pearsonr(xr, yr)[0]
        print('Rotation angle:',theta, 'Pearson Correlation:', p)
        str = 'Correlation: %.3f'%p
        plt.scatter(xr, yr, s=40, alpha=0.9, lw=0.5,c=clrs[i], marker='o',label=str)
    plt.legend(loc='upper left', shadow=True)
    plt.xlabel('X')
    plt.ylabel('Y')
    plt.title('Pearson correlation and distribution:%s'%tip, fontsize=18)
    plt.grid(b=True)
    plt.show()
```

Test the intro function:
```
np.random.seed(0)
intro()
```

Straight line function:
```
N=1000
tip = 'Straight line function'
x = np.random.rand(N)
y = np.zeros(N) + np.random.randn(N)*0.001
pearson(x, y, tip)
```

Positive-x half second-order function:
```
tip = 'Second order function'
x = np.random.rand(N)
y = x **2 + np.random.rand(N)*0.002
pearson(x, y, tip)
```

Tangent function:
```
tip = 'Tan function'
x = np.random.rand(N) * 1.4
y = np.tan(x)
pearson(x, y, tip)
```

Second-order function:
```
tip = 'Second order function'
x = np.linspace(-1, 1, 101)
y = x**2
pearson(x, y, tip)`
```

Draw eclipse. Scatter the points all over and select only those satisfying an eclipse.
```
tip = 'eclipse'
x, y = np.random.rand(2,N) *60 -30
y/=5
idx = (x**2 / 900 + y**2 / 36 < 1)
x = x[idx]
y = y[idx]
pearson(x, y, tip)
```
