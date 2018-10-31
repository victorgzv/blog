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
```
<h3>Displaying an image with OpenCV</h3>

Now that we can read an image, our next step will be to display it.
```python
cv2.imshow("image", I) # the title and path to image
key = cv2.waitKey(0) # Waits for the next key to be pressed
cv2.destroyAllWindows() # Destroys the exact window name passed as an argument.
```

<h3>Saving an image with OpenCV</h3>

Now that we have our image. OpenCV’s cv2.imwrite function enables you to save images to the specified path.
```python
cv2.imwrite(‘newImage.png’,I)
```
