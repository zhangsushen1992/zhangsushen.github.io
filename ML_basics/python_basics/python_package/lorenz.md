## Lorenz Function
Import packages:
```
from scipy.integrate import odeint
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
```
Define lorenz function:
```
def lorenz(state, t):
  sigma = 10
  rho = 28
  beta = 3
  x, y, z = state
  return np.array([sigma*(y-x), x*(rho-z)-y, x*y-beta*z])
```
```
def lorenz_trajectory(s0, N):
  sigma = 10
  rho = 28
  beta = 8/3
  delta = 0.001
  s = np.empty((N+1, 3))
  s[0] = s0
  for i in np.arange(1, N+1):
    x, y, z = s[i-1]
    a = np.array([sigma*(y-x), x*(rho-z)-y, x*y-beta*z])
    s[i] = s[i-1] + a * delta
  return s
```
