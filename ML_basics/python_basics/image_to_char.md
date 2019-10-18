## Converting an Image to Characters

We make use of the PIL package:
```
import numpy as np
from PIL import Image
```
Then we set the image path and the height of the converted image:
```
image_file = 'C:/Users/sz299/Desktop/scenary.jpg'
height = 100
```
We open the image file, scale the image to the desired height and the proportional width. Resize original image using Image.ANTIALIAS to select values in the interval.
```
img = Image.open(image_file)
img_width, img_height = img.size
width = 2 * height * img_width // img_height
img = img.resize((width,height), Image.ANTIALIAS)
```
We convert the image to gray scale:
```
pixels = np.array(img.convert('L'))
```
We define character sequence that corresponds to different gray scales in an descending order. The original 256 numbers representing gray scales are now divide into 16 scales. 
```
chars = 'MNHQ$OC?7>!:-;. '
N = len(chars)
# 256 color levels divided into 16 classes, represented by each character in turn
step = 256 // N
```
Define result as an empty sequence initially, then add each character to the empty sequence based on the gray scale. Return a line breaker at the edge of the image.
```
result = ''
for i in range(height):
    for j in range(width):
        result += chars[pixels[i][j] // step]
    result += '\n'
```
Output the result to a text file:
```
with open('text.txt', mode='w') as f:
    f.write(result)
```
Open the text.txt file in the folder and we have the image to character result.
