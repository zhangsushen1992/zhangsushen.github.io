## Fourier Transform
Import packages:
```
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
```
Define the functions to perform Fourier Transform on. T is the number of repeating unit in the triangle wave, while size is the number of points within one unit of triangle wave. Tile function repeat the first argument T times.
```
def triangle_wave(size, T):
  t = np.linspace(-1, 1, size, endpoint=False)
  y = np.abs(t)
  y = np.tile(y, T) - 0.5
  x = np.linspace(0, 2 * np.pi * T, size*T, endpoint=False)
  return x, y
```
Similar to above, size is the number of points within one repeating unit and T is the number of repeating unit.
```
def sawtooth_wave(size, T):
  t = np.linspace(-1, 1, size)
  y = np.tile(t, T)
  x = np.linspace(0, 2*np.pi*T, size*T, endpoint=False)
  return x,y
```
