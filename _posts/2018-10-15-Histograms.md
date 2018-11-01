---
layout: post
title: Histograms
categories: [Image Processing ]
---

The human eye likes contrast in images. The bright regions should be really bright and the dark regions should be really dark.
By using histograms we can understand better the distribution of pixel values in an image. Reading histograms helps you seeing the threshold and decide what side is white and what is dark in an imge. A histogram basically tells you how many pixels you have for each possible pixel value.

We can apply histogram equalisation. This means manipulating the pixel values to make the image more pleasant.
In a grayscale image, the values range from 0 to 255. The X-axis  will contain values from 0 to 255, and the Y-axis will contain the number of pixels in the image for each value. In a dark image, the histogram will show values located at the beginning of the histogram (low values) and a quite narrow range of values (poor contrast). During histogram equalisation, we take the values of an image and strech them out to range from 0 to 255. By using all the available values, the image will have better contrast.

Histogram equalisation can be also applied to colour images. However, since RGB images have three channels it won't make sense to apply the equalisation technique directly on a RGB image. It needs to be apply to the intensity values withouth disturbing the color of the image. So, we will need to covert our colour image into one of the colour spaces that separtes intensity values, such as YUV, YCbCr, HSV etc.

During this lecture we were tasked with the following exercise where we had to use the openCV equalisation functions and plotting the histograms of a greyscale image.

![_config.yml]({{ site.baseurl }}/images/histograms_task.png)


<h3>How do I do it in OpenCV?</h3>

Here is the code. Enjoy!

```python
import numpy as np
import cv2
from matplotlib import pyplot as plt
from matplotlib import image as image
import easygui

# Opening an image from a file:
# I = cv2.imread("image.jpg")

# Opening an image using a File Open dialog:
# f = easygui.fileopenbox()
I = cv2.imread("Wartime.jpg")

plt.figure()
plt.subplot(2,2,1) #there will be two rows, two columns of plots, we will plot this first one in location 1
plt.imshow(I)

grey = cv2.cvtColor(I,cv2.COLOR_BGR2GRAY)
#values need to be unravelled from matrix form to a 1D array.
Values = grey.ravel()
plt.subplot(2,2,2)
plt.hist(Values,bins=256,range=[0,256]);

H = cv2.equalizeHist(grey)
plt.subplot(2,2,3)
plt.imshow(H, cmap='gray')

G= H.ravel()
plt.subplot(2,2,4)
plt.hist(G,bins=256,range=[0,256]);
plt.show()
````



