## Singular Value Decomposition
The idea is to convert an image to a matrix with RGB three channels (three matrices). Perform SVD on each matrix and extract the Sigma matrix. Keep first k values in the Sigma matrix and restore to original image by multiplying U, Sigma and V. 

First we import the packages:
```
import numpy as np
import os
from PIL import Image
import matplotlib.pyplot as plt
import matplotlib as mpl
from pprint import pprint
```

Then write the function to restore image using first k singular values:
```
def restore1(sigma, u, v, K):
    m = len(u)
    n = len(v[0])
    a = np.zeros((m,n))
    for k in range(K):
        uk = u[:, k].reshape(m,1)
        vk = v[k].reshape(1,n)
        a += sigma[k] * np.dot(uk, vk)
    a[a<0] = 0
    a[a>255] = 255
    # Alternatively we can use:
    # a = a.clip(0, 255)
    return np.rint(a).astype('uint8')
```

The above is a vectorised form. Alternatively we can write for-loop form:
```
# Using for loops instead of vectorisation
def restore2(sigma, u, v, K):
    m = len(u)
    n = len(v[0])
    a = np.zeros((m,n))
    for k in range(K):
        for i in range(m):
            a[i] += sigma[k] * u[i][k] * v[k]
    a[a<0] = 0
    a[a>255] = 255
    return np.rint(a).astype('uint8')
```

Convert an image to np.array:
```
A = Image.open('C:/Users/sz299/Desktop/scenary.jpg')
print(A)
output_path = r'.\SVD_Output'
if not os.path.exists(output_path):
    os.mkdir(output_path)
a = np.array(A)
print(a.shape)
```

Find the SVD result from linalg:
```
K = 50
# Find the singular values in three channels of RGB
u_r, sigma_r, v_r = np.linalg.svd(a[:, :, 0])
u_g, sigma_g, v_g = np.linalg.svd(a[:, :, 1])
u_b, sigma_b, v_b = np.linalg.svd(a[:, :, 2])
```

Restore the image by keeping first k singular values and output to a folder with path output_path. Print the first 12 images.
```
plt.figure(figsize=(11,9),facecolor='w')
for k in range(1, K+1):
    print(k)
    R = restore1(sigma_r, u_r, v_r, k)
    G = restore1(sigma_g, u_g, v_g, k)
    B = restore1(sigma_b, u_b, v_b, k)
    I = np.stack((R,G,B),axis=2)
    Image.fromarray(I).save('%s\\svd_%d.png'%(output_path, k))
    if k <= 12:
        plt.subplot(3,4,k)
        plt.imshow(I)
        plt.axis('off')
        plt.title('Singular value numbers: %d'%k)
    plt.suptitle('SVD and image', fontsize=20)
    plt.tight_layout(0.3, rect=(0,0,1,0.92))
    plt.show()
 ```
 
