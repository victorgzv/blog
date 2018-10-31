---
layout: post
title: Colour spaces using OpenCV
categories: [Image Processing ]
---
Today we covered the different colour spaces which are the values that are used to represent the pixels and describe the
visual appearance of the image.

The main colour spaces are:

* RGB stands for Red, Green, Blue. However, in openCV the order is BGR .This will drive you mad for a while until you get used to it.
* HSV stands for Hue, Saturation and Value (or Intensity).
* YUV contains information about the luminance (Y) and the chrominance (UV) of an image. It can be very useful in improving the quality of an image.
* Grayscale represents the intensity of the image. A grayscale image has only one value per pixel, rather than three.
* Binary images can have only two values: black or white.


Using the cvtColor function we can switch to other colour spaces from RGB (BGR in openCV)
```python
RGB = cv2.cvtColor(I, cv2.COLOR_BGR2RGB)
HSV = cv2.cvtColor(I, cv2.COLOR_BGR2HSV)
YUV = cv2.cvtColor(I, cv2.COLOR_BGR2YUV)
G = cv2.cvtColor(I, cv2.COLOR_BGR2GRAY)
````
The following task shows how to convert and image to a different colour space, extract one of its colour channels and how to use mathplot lib and the subplot function.

```python
# import the necessary packages:
import cv2
import cv2
from matplotlib import pyplot as plt
from matplotlib import image as image

# Opening an image from a file:
I = cv2.imread("airballoons.jpg")
#Task 2 - Convert Image to YUV color space
image1YUV = cv2.cvtColor(I, cv2.COLOR_BGR2YUV)

#Task 3 & 4 - Extract Y channel & display using OpenCV's "imshow" function
image1Y = image1YUV[:,:,0]
cv2.imshow("image1_Y", image1Y)

#Task 5 - Convert original image to grayscale
image1Gray = cv2.cvtColor(I, cv2.COLOR_BGR2GRAY)
cv2.imshow("image1_Gray", image1Gray)
key = cv2.waitKey(0)

#Task 6 - Display these two on screen side by side using subplots
fig = plt.figure(1)
# Creating one row of 2 columns
# Placing this image in the first postion
fig.add_subplot(1,2,1)
plt.imshow(image1Y, 'gray')

# Placing this image in the second postion
fig.add_subplot(1,2,2)
plt.imshow(image1Gray, 'gray')

# Display plot
plt.show()

````
I hope you find this useful :)
