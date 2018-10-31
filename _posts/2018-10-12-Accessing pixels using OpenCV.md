---
layout: post
title: Accessing pixels using OpenCV
categories: [Image Processing ]
---
In this week lecture we looked at pixels.A pixel is the smallest unit of a digital image. Accessing pixels can be useful to to change the characteristics of an specific pixel or a set of pixels.

There are 5 pieces of information to identify a pixel: I, J and RGB.
The format of a pixel looks as follows: I[i,j,c]

* i is the row number and is equivalent to y
* j is thecolumn number and is equivalent to x
* c represents the colour: 0 blue, 1 green and 2 red.
(Remember RGB in opencv is BGR)

We also learned that we can apply values to a pixel in different ways.
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
To test the skills we just adquired we were given a new task. This task consisted of capturing the user input to access the location where a user has clicked using mouse events. To do this the code below shows how to implement a mouse callback function that captures left click events.

Once the user clicks are capture we needed to draw a 201 x 201, 5-pixel thick red square around this location, to then convert the pixels within the square to YUV colour space withouth falling of the edges of the image.

For this task I used this image that you can download.

![_config.yml]({{ site.baseurl }}/images/airballoons.jpg)

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
# handling when the user clicks the edges of the image
#size[1] is the width of an image
#size[0] is the height of an image
        if(x>101 and y>101 and x < size[1]-101 and y < size[0]-101 ):
            fit =original.copy() 
            # Print red square at the location selected
            cv2.rectangle(img = fit, pt1 = (pt1x,pt1y), pt2 =(pt2x,pt2y), color = (0,0,255), thickness = 5)
            #Switch seleted pixels to YUV
            YUV = cv2.cvtColor(fit, cv2.COLOR_BGR2YUV)
            fit[pt1y:pt2y,pt1x:pt2x,0]=YUV[pt1y:pt2y,pt1x:pt2x,0]

            cv2.imshow("image", fit)


cv2.namedWindow("image")
#Callback to draw function 
cv2.setMouseCallback("image", draw)
I = cv2.imread("airballoons.jpg")


size = np.shape(I) # size of the image
original = I.copy() #create a copy
cv2.imshow("image", I)
key = cv2.waitKey(0)
````
This is the output of the code above:

![_config.yml]({{ site.baseurl }}/images/pixels_task.png)
