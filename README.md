# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./intermittent_images/greyscale_solidwhite_right.jpg "Grayscale"
[image2]: ./intermittent_images/blur_solidwhite_right_image.jpg "Blurred"
[image3]: ./intermittent_images/canny_solidwhite_right_image.jpg "Canny"
[image4]: ./intermittent_images/masked_solidwhite_right_image.jpg "Masked"
[image5]: ./test_images_output/solidWhiteRightoutput.png  "Weighted"

---

### Reflection

My pipeline consisted of 5 steps. 
1. I converted the image to greyscale.
![alt text][image1]
2. Applied guassian blur to smoothen the edges.
![alt text][image2]
3. Applied canny edge dectection functionality on smoothened image.
![alt text][image3]
4. Applied pollyfit region of interest and discarded all the other lines outside this region of interest.
![alt text][image4]
5. Applied Hough Transform to detect lanes within the region of interest and two smooth red lines are drawn by interpolating the seperate lanes.
![alt text][image5]

In order to draw a single line on the left and right lanes, I added two new funtions called draw_each_line() and draw_both_lane_lines().

To create two solid lines instead of segmented lines, I first seperate left lane lines from right lane lines using the condition where left lane line will be negative. Later I'm interpolating the points by minimizing distance between these points using stats.linregress(x,y) from which we obtain slope and intercept of the lane as this helps us to reduce least square errors. 

At first solidYellowLeft.mp4 was showing distorted line at the right line of the lane. Later I discarded lines with smaller slopes as they might be forming horizontal lines thus obtaining smooth lines at both sides of the lanes without any distortion.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be that the hyperparameters are hard coded including the vertices for the polygon as it is assumed that camera position, angles are fixed and the road/lane is flat. So this method may not work on advance scenerios where there may be uphill/downhill or any other obstacles/vechicles in the way of our car.

Another shortcoming could be that straight lines doesn't work well on curved lanes.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use higher order polynomial without using more computational resources.

Another potential improvement could be to use deep learning model in order to find lanes thus eliminating hard coded parameters. This approach may also be improvised to detect lanes under various scenerios without fixed assumptions.
