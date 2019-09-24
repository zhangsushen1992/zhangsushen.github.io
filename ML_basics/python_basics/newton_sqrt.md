## Solution to the Square Root using Newton Method

We derive the Newton's method from the Taylor expansion: f(x) = f(x<sub>0</sub>) + f'(x<sub>0</sub>)(x-x<sub>0</sub>) + <sup>f''(x0)</sup>&frasl;<sub>2</sub>(x-x<sub>0</sub>)<sup>2</sup> + ...

We define g(x) = f'(x<sub>0</sub>) + f''(x<sub>0</sub>)(x -x<sub>0</sub>). At the minimum point, g(x) = 0. 

Therefore we can write the update rule as x = x<sub>0</sub> + f'(x<sub>0</sub>)/f''(x<sub>0</sub>). This is the update rule for the Newton method.

For the square root value of a number a, we define √a = x, a = x<sup>2</sup>, x<sup>2</sup> - a = 0. We define f(x) = x<sup>2</sup> - a. To find values where f(x) = 0, we seek to optimise F(x), which is the integral of f(x). We applly the Newton search on F(x).

Therefore, we have F'(x<sub>0</sub>) = f(x) = x<sup>2</sup> - a, and f''(x<sub>0</sub>) = 2x. The update rule is, thus, x = x/2 - a/(2x).

Let's now implement in code. First, we import all the modules.
```
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
import math
```

We then define the square root function using Newton's method, starting with x<sub>0</sub>=a/2.
```
def func(a):
  if a < 1e-6:
    return 0
  last = a
  x = a / 2
  while math.fabs(c - last) > 1e-6:
    last = x
    x = x / 2 + a / (2 * x)
  return x
```

The first if loop checks whether the defined value is too small to be calculated. The while loop solves the square root until the update value is small, i.e. reaching the minimum point. The most important update rule is reflected as:
```
x = x / 2 + a / (2 * x)
```

We plot a figure of square root function y = √x based on the function:
```
if __name__='__main__':
  x = np.linspace(0, 30, num=50)
  func_ = np.frompyfunc(func, 1, 1)
  y = func_(x)
  
  plt.figure(figsize=(10,5), facecolor = 'w') #sets the figure size and the background colour
  plt.plot(x, y, 'ro-', lw=2, markersize=6)
  plt.grid(b=True, ls=':')
  plt.xlabel('X', fontsize=16)
  plt.ylabel('Y', fontsize=16)
  plt.title('Square Root Function', fontsize=18)
  plt.show()
```
