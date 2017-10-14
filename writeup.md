# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/gray.jpg "Grayscale"
[image2]: ./test_images_output/edges.jpg "Canny Edge Detection"
[image3]: ./test_images_output/roi_edges.jpg "ROI Edges"
[image4]: ./test_images_output/solidYellowLeft_new.jpg "Final Result"
[image5]: ./test_images_output/solidYellowLeft_old.jpg "Before filtering, averaging, extrapolation"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. 
1) Grayscale image conversion, I used the default parameters for RGB to grayscale image conversion
![alt text][image1]
2) Gaussian smoothing / blurring is applied on grayscale image, images or video seems also not so sharp so kernel of size 3 was selected
3) Canny Edge detection after smoothing, here the parameters chosen were low_threshold = 100 and high_threshold = 200
![alt text][image2]
4) Defining Region of Interest within the image dimention,  by looking at the videos it was identified better to use a polygon, the parameters were adjusted till we get best results
![alt text][image3]
5) Performing Hough transformation for lines detection on edges available only in region of interest. With following parameters rho = 1, theta = np.pi/180, threshold = 30, min_line_len = 10, max_line_gap = 150, I was able to extract the lines marking the position of lanes and filter all the unnecessary lines within the image.
6) Then by using the weighted combining method the detected laned were plotted over original image
![alt text][image4]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by slope filtering.
As we know the slope for the right side should be negative and for the left side slope should have positive value. The range have been defined separately for right and left lanes. At the end average slope and average y-intercept is calculated for left aand right lanes. A single line has been drawn for by taking the average slope and y-intercept within the region of interest for both left and right lane separately
combining slopes together
![alt text][image5]
![alt text][image4]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
