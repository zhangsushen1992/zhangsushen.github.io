## Convolve Images
Import packages:
```
import numpy as np
import os
from PIL import Image
```
The convolve function times a filter area with the weight and slides through the whole image.
```
def convolve(image, weight):
  height, width = image.shape
  h, w = weight.shape
  height_new = height - h + 1
  width_new = width - w + 1
  image_new = np.zeros((height_new, width_new), dtype=np.float)
  for i in range(height_new):
    for j in range(width_new):
      image_new[i,j] = np.sum(image[i:i+h, j:j+w] * weight)
  image_new = image_new.clip(0,255)
  image_new = np.rint(image_new).astype('uint8')
  return image_new
```
Load the image and define the output path:
```
A = Image.open("..\\son.png", 'r')
output_path = '.\\ImageConvolve\\'
if not os.path.exists(output_path):
  os.mkdir(output_path)
```
Convert image into an array. Define kernels of avg3, avg5, gauss, soble_x, soble_y, soble, prewitt_x, prewitt_y, prewitt, laplacian4, laplacian8:
```
a = np.array(A)
avg3 = np.ones((3,3))
avg3 /= avg3.sum()
avg5 = np.ones((5,5))
avg5 /= avg5.sum()
gauss = np.array(([0.003, 0.013, 0.022, 0.013, 0.003],
                  [0.013, 0.059, 0.097, 0.059, 0.013],
                  [0.022, 0.097, 0.159, 0.097, 0.022],
                  [0.013, 0.059, 0.097, 0.059, 0.013],
                  [0.003, 0.013, 0.022, 0.013, 0.003]))
soble_x = np.array(([-1, 0, 1], [-2, 0, 2], [-1, 0, 1]))
soble_y = np.array(([-1, -2, -1], [0, 0, 0], [1, 2, 1]))
soble = np.array(([-1, -1, 0], [-1, 0, 1], [0, 1, 1]))
prewitt_x = np.array(([-1, 0, 1], [-1, 0, 1], [-1, 0, 1]))
prewitt_y = np.array(([-1, -1,-1], [0, 0, 0], [1, 1, 1]))
prewitt = np.array(([-2, -1, 0], [-1, 0, 1], [0, 1, 2]))
laplacian4 = np.array(([0, -1, 0], [-1, 4, -1], [0, -1, 0]))
laplacian8 = np.array(([-1, -1, -1], [-1, 8, -1], [-1, -1, -1]))
weight_list = ('avg3', 'avg5', 'gauss', 'soble_x', 'soble_y', 'soble', 'prewitt_x', 'prewitt_y', 'prewitt', 'laplacian4', 'laplacian8')
```
Separately convolve RGB channels and stack up to form one image in output path:
```
for weight in weight_list:
  R = convolve(a[:,:,0], eval(weight))
  G = convolve(a[:,:,1], eval(weight))
  B = convolve(a[:,:,2], eval(weight))
  I = np.stack((R,G,B), 2)
  Image.fromarray(I).save(output_path + weight + '.png')
```
