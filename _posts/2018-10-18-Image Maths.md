---
layout: post
title: Image Maths
categories: [Image Processing ]
---

This week in Image Processing we covered image transformations (scaling,cropping and rotating images) and image mathmatics.
For this post I will focus on the maths that can be applied on images. We can use arithmetic operations on images including addition, subtraction, multiplication and division.
For the following operations the images should be the same size.

* Addition:

```python
A = cv2.add(I1,I2)
# By default, OpenCV will saturate the output at 255.
````
* Subtraction:

```python
S = cv2.subtract(I1,I2)
# By default, OpenCV will saturate the output at 0.
````
* Multiplication:

```python
M = cv2.multiply(I1,I2,scale = 0.01)
# By default, OpenCV will saturate the output at 255.
# The scale parameter can be used to scale instead
````
* Division:

```python
D = cv2.divide(I1,I2,scale = 100)
# By default, OpenCV will saturate the output at 0.
# The scale parameter can be used to scale instead
````

A variation of the addition function is the addWeighted function that enables you to achieve the same
by calculating the weighted sum of two arrays.

```python
A = cv2.addWeighted(Image1,0.7,Image2,0.3,0)
# The second and forth parameters are the weight of 
# the first and second array elements of each image.
````

![_config.yml]({{ site.baseurl }}/images/histograms_task.png)

The following task shows how to scale images and adding them up to get a composite image, as well as how to use the addWeighted function.

<h3>How do I do it in OpenCV?</h3>

```python
import cv2
from matplotlib import pyplot as plt
from matplotlib import image as image
import numpy as np

o = cv2.imread("orange.png")
w = cv2.imread("water.jpg")

h1,w1= o.shape[:2]
h2,w2= w.shape[:2]
res1 = cv2.resize(o, dsize=(w1/2, h1/2))
res2 = cv2.resize(w, dsize=(w2/2, h2/2))

#Addition:
# A = cv2.add(res1,res2)
A = cv2.addWeighted(res1,0.7,res2,0.3,0)

h3,w3= A.shape[:2]
res3 = cv2.resize(A, dsize=(w3*2, h3*2))
cv2.imshow('image',res3)

key = cv2.waitKey(0)
````
