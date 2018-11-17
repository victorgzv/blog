---
layout: post
title: Skin Detection | Team Project
categories: [Image Processing Project]
---
In the first blog post of this Face Detection Project, I explained that one of our initial ideas to solve this problem was to isolate the skin colour regions of the people appearing on an image.

As we have made some progress researching and applying image classifiers, in this post I will focus on skin detection and the OpenCV contours functions.

In the last two weeks we had a few lectures in which we covered colour segmentation. This topic was really useful in order to complete the “Where is Wally” assignment. At this point I feel more comfortable to explore new methods and apply them to our face detection project.

To segment out skin colours from an image we will need to use the Hue Based colour space 
 (Hue, Saturation, and Value) and also one of the many Luminance based colour space (YCBCr, YIQ, and YUV). Using HSV is usually much easier to pick a desired colour as compared to using RGB. For example the skin in channel H is characterized by values between 0 and 50 and in the channel S from 0.23 to 0.68 for Asian and Caucasian ethnics.

YCbCr is a commonly used color space in digital video domain. Because its representation makes it easy to get rid of some redundant color information and it is a very useful space for skin detection.

The following code shows how I converted the graduation picture to both colour spaces and create a mask for each of them. Each mask ranges between a threshold of skin lower and upper values of skin colours. Finally both mask where added together to obtain better results.

```python
I = cv2.imread("Graduation.jpg")
HSV = cv2.cvtColor(I,cv2.COLOR_BGR2HSV)

hsv_lower_thresh = (0, 10, 60)
hsv_upper_thresh = (20, 150, 255)

#Threshold on HSV
msk_hsv = cv2.inRange(HSV, hsv_lower_thresh, hsv_upper_thresh)
# final_mask = cv2.bitwise_and(I, I, mask=msk_hsv) #Applying mask to the original image

#Threshold on YCbCr
YCbCr = cv2.cvtColor(I, cv2.COLOR_BGR2YCR_CB)
YCbCr_lower_thresh = (0, 138, 67)
YCbCr_upper_thresh = (255, 173, 133)
msk_YCbCr = cv2.inRange(YCbCr, YCbCr_lower_thresh, YCbCr_upper_thresh)

#Adding up both masks together
mask_image = cv2.add(msk_hsv,msk_YCbCr)

````
![_config.yml]({{ site.baseurl }}/images/skin_mask.png)


We need to take extra care with very dark parts of the image and probably discard them altogether, as the HSV conversion gets really noisy for small values of V. To achieve it, we need to apply morphological operations such as erode and dilate which will get rid of the noise in the image. Erosion removes the boundary pixels and dilation increases object boundary to background. This way, we can make sure whatever region in background in result is really a background, since boundary region is removed. [1]



````python
#morphological operations
#remove noise
image_foreground = cv2.erode(mask_image,None,iterations = 2)     

#The background region is reduced a little because of the dilate operation
dilated_binary_image = cv2.dilate(mask_image,None,iterations = 2)   

#set all background regions to 128 using a threshold
ret,image_background = cv2.threshold(dilated_binary_image,1,158,cv2.THRESH_BINARY)  

#add both foreground and background
marker = cv2.add(image_foreground,image_background)
#convert to 32SC1 format
marker32 = np.int32(marker)
````

![_config.yml]({{ site.baseurl }}/images/markers.png)

Watershed algorithm  identifies those regions we don’t know if they are background or the desired skin colour. After using this function we know which areas are skin regions and which are background. So we have created marker (it is an array of same size as that of original image, but with int32 datatype) and label the regions inside it. [ 1]

```python
cv2.watershed(I,image_marker32)
m = cv2.convertScaleAbs(image_marker32) #convert back to uint8
#bitwise of the mask with the input image
ret,image_mask = cv2.threshold(m,0,255,cv2.THRESH_BINARY+cv2.THRESH_OTSU)
output = cv2.bitwise_and(I,I,mask = image_mask)
````
![_config.yml]({{ site.baseurl }}/images/skin_detection1.png)

Unfortunately, the graduation image has a background similar to the range of skin colours that we want to detect. This algorithm will be test with different images where the background is completely different.

Finally, using contours functions on the masked image we can extract those areas that contain skin. (This may also include hands and legs). By using boundinRect, a rectangle can we drawn around each coordinate of the found contours

The result of this function have been applied to a different image where the background is all green giving us better results.


```python

def findFaces():
    _,contours,h= cv2.findContours(image_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    for i in range(len(contours)):
        contours = sorted(contours, key=cv2.contourArea,reverse=False)#sorting contours
        L = cv2.arcLength(contours[i] ,True)#length of each contour
        
        x,y,w,h=cv2.boundingRect(contours[i])
        cv2.rectangle(I,(x,y),(x+w+10,y+h+5),(10,0,255),2)

````
![_config.yml]({{ site.baseurl }}/images/skin_detection_rugby.png)

The future scope of this algorithm will be to detect faces.


<h3>References:</h3>
[1] https://docs.opencv.org/3.4.3/d3/db4/tutorial_py_watershed.html

https://courses.cs.washington.edu/courses/cse576/18sp/notes/FaceDetRec18.pdf

http://www.robots.ox.ac.uk/~vgg/research/hands/
