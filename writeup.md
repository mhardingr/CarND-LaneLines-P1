# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_output_examples/test_images_output/full-line-labeled-whiteCarLaneSwitch.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 
My lane-finding pipeline consisted of 5 steps: 1) Convert a copy of the image to grayscale, 2) 
apply gaussian blur (kernel size 5) the image to smooth insignficant edges, 3)
apply the Canny image filter with medium threshold values, 4) mask out the ROI trapezoid
contained in the bottom half of the image, 5), extract line segments with at least 35 intersections
in hough parameter space of the masked image, 6) draw an averaged and extrapolated 
line for each of left and right lanes, and 7) overlay labeled lanes image on top of
original imgae.

In order to draw a single line on the left and right lanes, I modified the draw_lines function 
to collect average values of the slope (m) and offset (b) for the left and right lanes. A line 
segment pertained to the left lane when its slope was determined to be non-negative, else it 
belonged to the right lane. I solved for the x coordinates for the extrapolated line for each lane
by requiring first that the y coordinates must be the bottom and middle of the image.

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


As seen when processing the challenge image, my pipeline does not seem robust to
lanes with severe shadowing or high curvature.
When severe shadowing is present, it seems that fewer line segments are found and there
is more noise in what the computed slope and offset values are for these line segments. This 
may be the source of "divide by zero" errors my code runs into because it seems that the right lane
almost but disappears under the tree shadow.
When high lane curvature is present, I would expect my pipeline's extrapolated 2D lane line to
not be a good fit.


### 3. Suggest possible improvements to your pipeline

My line segment filtering algorithm is too brittle and sensitive to lighting conditions, which
leads to lack of robustness to the tree's shadowing the right lane segments in the challenge video.
As suggested in the future lessons, thresholding over a more lighting-invariant color space would be
beneficial.

To address high lane curvature, we could remove the assumption that lanes must be estimated
to be 2D lines and instead use polynomial regression to estimate a low-degree polynomial to better
fit the actual curvature observed.
