---
layout: post
title: Simple operations on images using OpenCV
categories: [Image Processing]
---
Hi there!

My name is Victor Gonzalez and I'm a Computer Science student. This week, we had our first lecture in Image Processing and we set up Python and OpenCV.

As I am planning on using Image processing in my final year project I’ve started going through the lecture notes available on the college platform and tried out some tasks.

In this blog I will illustrate the progress made week by week on this module.Let's get started ...

Using OpenCV, we can read an image, display it and after we are done with our operations, save it.

<h3>Reading an Image in OpenCV</h3>

We will use the OpenCV function cv2.imread(). The function cv2.imread() requires two arguments: the first is the path to the image itself and the second specifies the way the image should be read.

```python
import cv2

# Opening an image from a file:
I = cv2.imread("airballoons.jpg")
````
