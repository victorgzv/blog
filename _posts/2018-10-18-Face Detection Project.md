---
layout: post
title: Face Detection Team Project
categories: [Image Processing Project]
---
To date I have done some research on this topic. I came across using different classifiers in order to detect region of faces. There are two widely used classifiers called Haar cascade classifiers and Local Binary Patterns. In this post I will talk about Haar feature-based cascade classifier.

But first, let me explain what is a classifier. A classifier is the combination of a feature and its corresponding learned threshold value. A feature applied to an image sample results in a value that gets compared to a threshold. [1] Anything below that threshold is negative (no detection), and anything above it is positive, e.g., the classifier decides the object is present in the sample image.

Strong classifiers are then arranged in a cascade. Commonly, they are arranged according to their number of weak classifiers, ascending in order. Modern object recognition has made large advancements as the machine learning techniques have been incorporated.

<h3>Haar Cascade</h3>

Paul Viola and Michael Jones invented an efficient algorithm for face recognition [2]. They described a visual object detection framework that was faster, more accurate, and more robust than anything at the time. This framework was well-received, and since then, many object detection algorithms, as well as many action detection and recognition algorithms, have been built off of the Viola-Jones system. 

OpenCV comes with Haar cascade classifiers for face recognition.[3] This are essentially XML files which contain labelled face features that have been learned and pre trained by an algorithm.

OpenCV allows you to train your own classifiers. In this project we will use the haarcascade_frontalface_default.xml file. These files can be located in the OpenCV root folder.
The algorithm starts by extracting Haar features from each image. It involve many steps where a box is placed over the picture to calculate a feature. This process consists on subtracting the sum of pixels under the white area of the box from the sum of pixels under the black area of the box.

Finally it discard irrelevant features and keeps those relevant technique called Adaboost. AdaBoost is a training process for face detection, which selects only those features known to improve the classification (face/non-face) accuracy of our classifier.[4]

![_config.yml]({{ site.baseurl }}/images/haar.png)

Let’s see how we applied this idea. The following code is an example from the opencv documentation. 

```python

import numpy as np
import cv2 as cv
faces = face_cascade.detectMultiScale(gray, 1.3, 5)
for (x,y,w,h) in faces:
cv.rectangle(img,(x,y),(x+w,y+h),(255,0,0),2)
cv.imshow( ‘img’,img)
cv.waitKey(0)
cv.destroyAllWindows()
````
<h3>References</h3>

[1] M. J. Laielli, "Multi-Frame Object Detection," Naval Postgraduate School, Monterey, California, 2012. 
[2] P. Viola and M. J. Jones, Rapid Object Detection using a Boosted Cascade of Simple Features, Cambridge: Conference on Computer Vision and Pattern Recognition 
[3] https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_objdetect/py_face_detection/py_face_detection.html#face-detection
[4] https://en.wikipedia.org/wiki/AdaBoost

