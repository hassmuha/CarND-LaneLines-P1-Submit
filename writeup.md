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
2) Gaussian smoothing / blurring is applied on grayscale image, images or video seems not so sharp so kernel of size 3 was selected
3) Canny Edge detection after smoothing, the following parameters were chosen : low_threshold = 100 and high_threshold = 200
![alt text][image2]
4) Defining Region of Interest within the image dimension,  by looking at the videos it was identified better to use a polygon, the parameters were adjusted till we get best results
![alt text][image3]
5) Performing Hough transformation for lines detection on edges available only in region of interest. With following parameters : rho = 1, theta = np.pi/180, threshold = 30, min_line_len = 10, max_line_gap = 150, I was able to extract the lines marking the position of lanes and filter all the unnecessary lines within the image.
6) Then by using the weighted combining method the detected lane lines were plotted over original image
![alt text][image4]


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by slope filtering and averaging. The slope filtering range have been defined separately for right and left lanes. All the lines who lie in those ranges either correspond to left or right lane. At the end average slope and average y-intercept is calculated for left and right lanes. A single line has been drawn by taking the average slope and y-intercept within the region of interest for both left and right lane separately.
![alt text][image5]
![alt text][image4]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen if in some images of a video, edge detection fails due to too much reflection of light or shadowing effect on the road. If the edges have not been detected that means no lanes or wrong lanes have been detected.

Another shortcoming could be selection of region of interest based on different cameras view. Currently based on the two sample videos the optimum region of interest has been identified but this would not work perfectly with change in camera view.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have a prediction mechanism to be deployed which captures lane detection information from previous images in a video stream to detect the optimum position of lanes in the corresponding image. This would also help to make decision during the time when any block fails to detect the lanes but based on previous history we can safely mark the lanes.

Another potential improvement could be based on the camera calibration and field of view parameters for region of interest should be selected.
