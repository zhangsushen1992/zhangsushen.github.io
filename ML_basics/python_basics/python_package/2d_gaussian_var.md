## Effects of Variance on Gaussian Distribution
Import packages:
```
import numpy as np
from scipy import stats
import matplotlib as mpl
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
```
Draw a meshgrid:
```
# Similar to meshgrid, using mgrid. Check also ogrid
# j means the number of points between -5 and 5, without j is the step length
x1, x2 = np.mgrid[-5:5:51j, -5:5:51j]
x = np.stack((x1,x2),axis=2)
```
Define different variances in sigma. Define funciton using stats.multivariate_normal with sigma. y is norm.pdf(x). Then plot.
```
plt.figure(figsize=(9,8), facecolor='w')
sigma = (np.identity(2), np.diag((3,3)), np.diag((2,5)), np.array(((2,1),(1,5))))
for i in np.arange(4):
    ax = plt.subplot(2,2,i+1, projection='3d')
    norm = stats.multivariate_normal((0,0), sigma[i])
    y = norm.pdf(x)
    ax.plot_surface(x1, x2, y ,cmap=cm.Accent, rstride=2, cstride=2, alpha=0.5, lw=0.3)
    ax.set_xlabel('X')
    ax.set_ylabel('Y')
    ax.set_zlabel('Z')
plt.suptitle('2D Gaussian Std Comparison', fontsize=10)
plt.tight_layout(1.5)
plt.show()
```
