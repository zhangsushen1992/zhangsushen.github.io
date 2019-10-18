## Calculating Stats Parameters and Draw Normal Distribution
First, import packages:
```
import numpy as np
from scipy import stats
import math
import matplotlib as mpl
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
import matplotlib.pyplot as plt
import seaborn
```
Next, define a function that calculates parameters from raw number or from package.
```
def calc_statistics(x):
    n = x.shape[0]
    m = 0
    m2 = 0
    m3 = 0
    m4 = 0
    for t in x:
        m += t
        m2 += t*t
        m3 += t**3
        m4 += t**4
    m /= n
    m2 /= n
    m3 /= n
    m4 /= n
    
    mu = m
    sigma = np.sqrt(m2 - mu * mu)
    skew = (m3 - 3*mu*m2 + 2*mu**3) / sigma**3
    kurtosis = (m4 - 4*mu*m3 + 6*mu**3*mu + mu**4) / sigma**4 -3
    print('Average, Standard deviation, Skewness and Kurtosis are:', mu, sigma, skew, kurtosis)
    
    mu = np.mean(x, axis=0)
    sigma = np.std(x, axis=0)
    skew = stats.skew(x)
    kurtosis = stats.kurtosis(x)
    
    return mu, sigma, skew, kurtosis
```
Randomly select 10000 points from normal distribution using random.randn(). Calculate parameters.
```
d = np.random.randn(10000)
print(d)
print(d.shape)
mu, sigma, skew, kurtosis = calc_statistics(d)
```
Plot the results in histogram to see overall distribution.
```
plt.figure(num=1, facecolor='w')
# x1 is the list of all input values, y1 is the height of each column in histogram
y1, x1, dummy = plt.hist(d, bins=50, density=True, color='g', alpha=0.75)
print(y1, x1, dummy)
t = np.arange(x1.min(), x1.max(), 0.05)
y = np.exp(-t**2/2) / math.sqrt(2*math.pi)
plt.plot(t, y, 'r-', lw=2)
plt.title('Gaussian distribution, sample number: %d'%d.shape[0])
plt.grid(True)
plt.show()
```

### 2D Gaussian
We then plot 2D Gaussian distribution. Select 100000 points in 2D. Calculate parameters using the function:
```
d = np.random.randn(100000,2)
mu, sigma, skew, kurtosis = calc_statistics(d)
print('Average, Standard deviation, Skewness and Kurtosis are:', mu, sigma, skew, kurtosis)
```
Then plot histogram with a surface plot:
```
N = 30
density, edges = np.histogramdd(d, bins=[N,N])
print('Total number of samples:', np.sum(density))
density /= density.max()
x = y = np.arange(N)
t = np.meshgrid(x,y)
fig = plt.figure(facecolor='w')
ax = fig.add_subplot(111, projection='3d')
# s is size, c is color
ax.scatter(t[0], t[1], density, c='r', s=50*density, marker='o', depthshade=True)
# rstride is row stride, cstride is column stride. they determine how far to draw a line
ax.plot_surface(t[0], t[1], density, cmap=cm.Accent, rstride=1, cstride=1, alpha=0.9, lw=0.75)
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
plt.title('2D Gaussian Distribution, total sample size: %d' %d.shape[0], fontsize=15)
plt.tight_layout(0.1)
plt.show()
```




