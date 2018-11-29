---
layout: post
title: Final Code | Team Project
categories: [Image Processing ]
---
The code bellow shows the final implementation of our project with the two approaches we took towards face detection.
1. Skin detection.
2. Haar cascades.
```python
# import the necessary packages:
import cv2
from matplotlib import pyplot as plt
from matplotlib import image as image
import easygui
import numpy as np
#---------------------------------------------------------------------------------------------------------------------------------------------
# SKIN SEGMENTATION APPROACH
#---------------------------------------------------------------------------------------------------------------------------------------------
def skinRange(I):

        HSV = cv2.cvtColor(I,cv2.COLOR_BGR2HSV)
        hsv_lower_thresh = (0, 10, 60)
        hsv_upper_thresh = (20, 150, 255)
        #Threshold on HSV
        msk_hsv = cv2.inRange(HSV, hsv_lower_thresh, hsv_upper_thresh)
        #Threshold on YCbCr
        YCbCr = cv2.cvtColor(I, cv2.COLOR_BGR2YCR_CB)
        YCbCr_lower_thresh = (0, 138, 67)
        YCbCr_upper_thresh = (255, 173, 133)
        msk_YCbCr = cv2.inRange(YCbCr, YCbCr_lower_thresh, YCbCr_upper_thresh)
        #Adding up both masks together
        mask_image = cv2.add(msk_hsv,msk_YCbCr)
        #morphological operations
        # kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(1,1))
        image_foreground = cv2.erode(mask_image,None,iterations = 2)#remove noise
        dilated_binary_image = cv2.dilate(mask_image,None,iterations = 2)#The background region is reduced
        ret,image_background = cv2.threshold(dilated_binary_image,1,128,cv2.THRESH_BINARY)  #setbackground to 128
        #add both foreground and backgroud markers.
        image_marker = cv2.add(image_foreground,image_background)
        #The markers are the future image regions.
        image_marker32 = np.int32(image_marker) #convert to 32SC1 format
        #Apply watershed
        cv2.watershed(I,image_marker32)
        m = cv2.convertScaleAbs(image_marker32) #convert back to uint8
        #bitwise of the mask with the input image
        ret,image_mask = cv2.threshold(m,0,255,cv2.THRESH_BINARY+cv2.THRESH_OTSU)
        output = cv2.bitwise_and(I,I,mask = image_mask)
        return output,image_mask
#---------------------------------------------------------------------------------------------------------------------------------------------
def detectSkin(image_mask):
    _,contours,h= cv2.findContours(image_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    for i in range(len(contours)):
        contours = sorted(contours, key=cv2.contourArea,reverse=False)#sorting contours
        L = cv2.arcLength(contours[i] ,True)#length of each contour
        x,y,w,h=cv2.boundingRect(contours[i])
        cv2.rectangle(I,(x,y),(x+w+10,y+h+5),(10,0,255),2)
#---------------------------------------------------------------------------------------------------------------------------------------------
# HAAR CASCADES APPROACH
#---------------------------------------------------------------------------------------------------------------------------------------------
def find_region(img,classifier,scaleFactor,minNeighbours,color,region_name):
    #scaleFactor-> some faces take more pixels, we can change this parameters to detect faces that are smaller
    #minNeighbors-> specify how many features we need to approve a region as a face or not
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    region = classifier.detectMultiScale(gray, scaleFactor, minNeighbours)
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
    coords_list , regionsNo = find_region(img,faceCascade, 1.05, 5, color["red"],"face")
    print("Faces found:", regionsNo)
#---------------------------------------------------------------------------------------------------------------------------------------------
#Calls to the Haar cascade functions
I = cv2.imread("Graduation.jpg")
copied= I.copy()

face_cascade = cv2.CascadeClassifier('classifiers/haarcascade_frontalface_alt2.xml')
detect(copied,face_cascade)
cv2.imshow('Haar cascade',copied)
#---------------------------------------------------------------------------------------------------------------------------------------------
#Calls to the Skin Segmentation functions
x,y= skinRange(I)
detectSkin(y)
cv2.imshow('mask',x)
cv2.imshow('Skin segmentation',I)
#---------------------------------------------------------------------------------------------------------------------------------------------
cv2.waitKey(0)
cv2.destroyAllWindows()

´´´´
