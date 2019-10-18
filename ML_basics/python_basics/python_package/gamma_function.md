## Plotting Gamma and Factorial Functions
Import packages:
```
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
from scipy.special import gamma
from scipy.special import factorial
```
Plot at 5 points (N=5). y is the gamma function of x+1, and z is the factorial function.
```
N = 5
x = np.linspace(0, N, 50)
y = gamma(x+1)
plt.figure(facecolor='w')
plt.plot(x, y, 'r-', x, y, 'mo', lw=2, ms=7, mec='k')

z = np.arange(0, N+1)
f = factorial(z,exact=True)
print(f)

plt.plot(z, f, 'go', markersize=9)
plt.grid(b=True)
plt.xlim(-1,N+0.1)
plt.ylim(0.5, np.max(y)*1.05)
plt.xlabel('X', fontsize=15)
plt.ylabel('Gamma(x)', fontsize=16)
plt.show()
```
Printing z gives:
- [  1   1   2   6  24 120]
