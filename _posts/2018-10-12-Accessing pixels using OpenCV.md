---
layout: post
title: Accessing pixels using OpenCV
categories: [Image Processing ]
---
In this week lecture we looked at pixels.A pixel is the smallest unit of a digital image. Accessing pixels can be useful to to change the characteristics of an specific pixel or a set of pixels.

There are 5 pieces of information to identify a pixel: I,J and RGB.
The format of a pixel looks as follows: I[i,j,c]

* i is the row number and is equivalent to y
* j is thecolumn number and is equivalent to x
* c represents the colour: 0 blue, 1 green and 2 red. (Remember RGB in opencv is BGR)

If we want to assign a value to a pixel:
```python
# This will maximise the intensity of the blue colour at that pixel location.
I[i,j,0]= 255

# The blue value at that location remember RGB in opencv is BGR
I[i,j] = (255,0,0) 

# The colon (:) allows us to select a range of values
#all the pixels in the rectangle 
#from (300, 100) to (400, 200)in blue.
I[100:200,300:400] = (255,0,0)
````

```python
import cv2
from matplotlib import pyplot as plt
from matplotlib import image as image
import easygui
import imutils
import numpy as np

def draw(event,x,y,flags,param):
    if event == cv2.EVENT_LBUTTONDOWN:
        pt1x = x-101
        pt1y = y-101
        pt2x = x+101
        pt2y = y+101

        if(x>101 and y>101 and x < size[1]-101 and y < size[0]-101 ):
            fit =original.copy()
            cv2.rectangle(img = fit, pt1 = (pt1x,pt1y), pt2 =(pt2x,pt2y), color = (0,0,255), thickness = 5)
            YUV = cv2.cvtColor(fit, cv2.COLOR_BGR2YUV)
            fit[pt1y:pt2y,pt1x:pt2x,0]=YUV[pt1y:pt2y,pt1x:pt2x,0]

            cv2.imshow("image", fit)



# Opening an image from a file:

cv2.namedWindow("image")
cv2.setMouseCallback("image", draw)
I = cv2.imread("airballoons.jpg")

resized = imutils.resize(I,width=1000,height=1000);
size = np.shape(resized)
original = resized.copy()
cv2.imshow("image", resized)
key = cv2.waitKey(0)
````
