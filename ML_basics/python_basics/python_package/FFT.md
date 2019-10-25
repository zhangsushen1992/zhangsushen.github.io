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
Define another triangular wave from the sawtooth wave. The frequency doubles from the first defintion.
```
def triangle_wave2(size, T):
  x, y = sawtooth_wave(size, T)
  return x, np.abs(y)
```
Define a non_zero function to remove signals that are almost zero.
```
def non_zero(f):
  f1 = np.real(f)
  f2 = np.img(f)
  eps = 1e-4
  return f1[(f1 > eps) | (f1 < -eps)], f2[(f2 > eps) | (f2 < -eps)]
```
Then we enter the main function. We use set_printoptions to print only in fixed form of float numbers, not in scientific form. x is the sampling of points from 0 to 2pi. y is the original signal:
```
np.set_printoptions(suppress=True)
x = np.linspace(0, 2*np.pi, 16, endpoint=False)
y = np.sin(2*x) + np.sin(3*x + np.pi/4) + np.sin(5*x)
```
N is the number of samples. f is the Fourier Transformed signal in the frequency domain. a is the strenght of frequency.
```
N = len(x)
f = np.fft.fft(y)
a = np.abs(f/N)
```
Reverse Fourier Transform and check whether the result are the same with the original functions. np.allclose checks whether two arrays are the same elementwise with a tolerance:
```
iy = np.fft.ifft(f)
print('imaginary part:', np.imag(iy))
print('real part:', np.real(iy))
print('whether same as original:', np.allclose(np.real(iy),y)
```
Plot figures. subplot(211) means 2 rows, 1 column, plot number 1. Plt.stem are graphs consisting of vertical lines with a marker on top. We plot stem graph of the frequncy domain signal:
```
plt.figure(facecolor='w')
plt.subplot(211)
plt.plot(x, y, 'go-', lw=2)
plt.title('Time domain signal', fontsize=15)
plt.gird(True)

plt.subplot(212)
w = np.arange(N) * 2 * np.pi / N
print('Frequency domain sampling:' w)
plt.stem(w, a, linefmt='r-', markerfmt='ro')
plt.title('Frequency domain signal', fontsize=15)
plt.grid(True)
plt.show()
```
We then input the triangle wave:
```
x, y = triangle_wave(20,5)
N = len(y)
f = np.fft.fft(y)
print('Original signal in frequency domain:', non_zero(f))
a = np.abs(f/N)
```
Remove all signals smaller than eps in the frequency domain, both real and imaginary part. Inverse transform the frequency back to time domain signal:
```
f_real = np.real(f)
eps = 0.1 * f_real.max()
f_real[(f_real < eps) & (f_real > -eps)] = 0
f_imag = np.imag(f)
eps = 0.3 * f_imag.max()
f_imag[(f_imag < eps) & (f_imag > -eps)] =0
f1 = f_real + f_imag *1j
y1 = np.fft.ifft(f1)
y1 = np.real(y1)
```
Last we plot the three graphs: original wave form, frequency domain signal, and filtered and restored wave form:
```
plt.figure(figsize=(8, 8), facecolor='w')
plt.subplot(311)
plt.plot(x, y, 'g-', lw=2)
plt.title('Triangular wave', fontsize=15)
plt.grid(True)

plt.subplot(312)
w = np.arange(N) * 2*np.pi / N
plt.stem(w, a, linefmt='r-', markerfmt='ro')
plt.title('Frequency domain signal', fontsize=15)
plt.grid(True)

plt.subplot(313)
plt.plot(x, y1, 'b-', lw=2, markersize=4)
plt.title('Restored triangular wave', fontsize=15)
plt.grid(True)

plt.tight_layout(1.5, rect[0, 0.04, 1, 0.96])
plt.suptitle('FFT and Frequency domain filter', fontsize=17)
plt.show()
```
