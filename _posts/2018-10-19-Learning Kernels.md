---
layout: post
title: Learning Kernels
categories: [Image Processing]
---
Yesterday we learned more about kernels.Image Kernels are a small matrix used to apply effects such as sharpening,outlining, gradient extraction, noise reduction,etc. There are the key for many image processing operations as they allow us to determine the most important parts of an image. This is an operator which makes each pixel a weighted sum of its
neighbours. This process is called convolution. See [convolutional neural networks.](https://en.wikipedia.org/wiki/Convolutional_neural_network) 

In the following example, the sharpen kernel is used to enhance differences in adjacent pixel values, making the image look more vivid.

```python
# import the necessary packages:
import cv2
from matplotlib import pyplot as plt
from matplotlib import image as image
import easygui
import numpy as np

# Opening an image 
I = cv2.imread("airballoons.jpg")
# resizing the image so it fits on the screen
h,w= I.shape[:2]
res1 = cv2.resize(I, dsize=(w/4, h/4))
# Converting image to grayscale
G = cv2.cvtColor(res1,cv2.COLOR_BGR2GRAY)
```
Until here there is nothing new, but in the next two lines we create an array which is our kernel and apply it to the image.
```python
k = np.array([[0,-1,0], [-1,5,-1], [0,-1,0]],dtype=float)
F = cv2.filter2D(G,ddepth=-1,kernel=k)
````
Finally display both images to see the difference
```python
cv2.imshow("image",G)
cv2.imshow("Sharpen image",F)
key = cv2.waitKey(0)
````
The result looks like this:

![_config.yml]({{ site.baseurl }}/images/kernels.png)

We can highlight the changes by subtracting the filtered image from the orginal one.

![_config.yml]({{ site.baseurl }}/images/subtract.png)


