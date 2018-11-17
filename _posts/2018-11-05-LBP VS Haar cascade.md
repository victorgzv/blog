---
layout: post
title: LBP VS Haar cascade | Team Project
categories: [Image Processing Project]
---
<h3>Local Binary Pattern</h3>
For the purpose of this project, we have also researched another classifier.
In this post I will discuss LBP (Local Binary Pattern)Cascades compared to Haar Cascade classifier.

Similar to Haar Cascades, LBP has also been trained with thousands of images. Each training image was divided into blocks. From this blocks the algorithm looks at 9 pixels at a time. This is a kernel (already explained in previous blogs) that compares the central pixel of the 3 by 3 block with its neighbours. If a neighbour pixel is greater than the middle pixel is set to 1as shown in the image below.

![_config.yml]({{ site.baseurl }}/images/LBP.png)

This process is done in a clockwise order. As a result, this technique return a binary number which is then converted to decimal giving value to the middle pixel. This is done to every pixel in a block.

Finally each block is converted into a histogram (also explained in previous posts) and combined with the rest of blocks we can obtain all the features we requires for face detection.

In terms of accuracy and speed, each classifier has its pros and cons.
When I tested it out with my images I obtained the following results. I used the same parameters in both classifier and recorded the time taken before and after calling the detect function.

From the following screenshot we can see that while LBP is significantly faster but less accurate thanm using Haar. (10-20% less accuarte).

Haar cascade is the winner and the most suitable classifier for our project!

![_config.yml]({{ site.baseurl }}/images/time_diff.png)
