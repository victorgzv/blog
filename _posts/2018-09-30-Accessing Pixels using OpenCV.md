---
layout: post
title: Accessing Pixels using OpenCV
categories: [Image Processing ]
---

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
fig.add_subplot(1,2, 1)
plt.imshow(image1Y, 'gray')

fig.add_subplot(1,2, 2)
plt.imshow(image1Gray, 'gray')

plt.show()

````
