---
layout: post
title: Applying Haar cascade classifier | Team Project
categories: [Image Processing Project]
---
In the previous post I talked about face detection using  Haar cascade classifier.
To date, my team mate Simon and I have implemented this approach on our project.
Using the Haar cascade classifier we managed to successfully identify and outline most of the faces from the graduation image.

To achieve this, we used an OpenCV function called <b>detectMultiScale</b>. This function allows us to process an image with a pre trained model of faces features ( e.g: <b>‘haarcascade_frontalface_default.xml’</b> ). In order for this function to work, the image passed has to be previously converted to grayscale. The reason for that is because detectMultiScale applies histogram equalisation to obtain an image with better contrast before applying the classifier.

This function determines the accuracy of face detection and takes two important parameters:

<b>scaleFactor:</b> Parameter specifying how much the image size is reduced at each image scale.
The model has a fixed size defined during training, which is visible in the xml of the classifier. 
Some faces will take more pixels. We can change this parameters to detect faces that are smaller.

<b>minNeighbours:</b> Parameter specifying how many neighbours each candidate rectangle should have to retain it. Basically, we can specify how many features we need to approve a region as a face or not.

Additionally, we could specify the <b>minSize</b> and <b>maxSize</b> of possible object size we want to detect. This could be useful to create a range of different face sizes.

The following script contains the code of our face recognition algorithm:

```python
# Face recognition
# (C) Image Processing Team Project 2018
# import the necessary packages:
import cv2
from matplotlib import pyplot as plt
from matplotlib import image as image
import easygui, time

def find_region(img,classifier,scaleFactor,minNeighbors,color,region_name):
    #scaleFactor-> some faces take more pixels, we can change this parameters to detect faces that are smaller
    #minNeighbors-> specify how many features we need to approve a region as a face or not
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    region = classifier.detectMultiScale(gray, scaleFactor, minNeighbors)
    coords =[]
    coords_list = []

    for (x,y,w,h) in region:
        cv2.rectangle(img,(x,y),(x+w, y+h),color,2)
        #cv2.putText(img,region_name,(x, y-4),cv2.FONT_HERSHEY_SIMPLEX,0.8,color,1,cv2.LINE_AA)
        coords = [x,y,w,h]
        coords_list.append(coords)
    #print the number of faces found
    regionsNo= len(region)
    return coords_list,regionsNo

def detect(img,faceCascade):
    color = {"red":(0,0,255),"green":(0,255,0),"blue":(255,0,0),"black":(0,0,0)}
    coords_list , regionsNo = find_region(img,faceCascade, 1.05, 6, color["red"],"face")
    print("Faces found:", regionsNo)
    return img

# Opening an image from a file:
I = cv2.imread("Graduation.jpg")

face_cascade = cv2.CascadeClassifier('classifiers/haarcascade_frontalface_default.xml')
detect(I,face_cascade)

cv2.imshow('img',I)
cv2.waitKey(0)
cv2.destroyAllWindows()
`````

These are the results obtained by using haar cascades. 18 faces out of 21 where found.
In the following weeks we will focus on improving this algorithm.

![_config.yml]({{ site.baseurl }}/images/project_output.png)

<h3>References:</h3>

Recommended values for detectMultiscale parameters OpenCV
https://stackoverflow.com/questions/20801015/recommended-values-for-opencv-detectmultiscale-parameters [Accesed October 28th]

