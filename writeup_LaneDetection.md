# **Finding Lane Lines on the Road** 


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"


### Reflection

### 1. The project pipeline

Pipeline to detect lane is implemented in the function lane_detection. It consists of the following steps:
1. Image is first converted to grayscale 
2. On the obtained grayscale image, gaussian blur is performed in order to remove noise and spurious gradient
3. Now, image is ready for Canny algorithm for edge detection. cv2.Canny() is used for Canny edge detection with a 
low threshold of 50 and high threshold of 150
4. Area of interest (i.e road area) is selected after this
5. Now, from the edges detected, lines are detected using Hough transform using 
cv2.HoughLinesP() with rho = 2, theta = pi/180, min_line_len = 20 and max_line_gap = 10.
These values are tuned for the images available
6. Once lines are detected, draw_lines() is modified to have two lines corresponding
to left line and right line. For this, lines with negative slope and positive slope are
seperated as left and right lines respectively. Average of slope and intercept
constant of all the left lines and right lines were found which was then passed
to connect_lines() function which then drew two lines corresponding to these slope 
and intercept values
7. Finally, weighted_img() combines the two images to provide the resultant final 
image


---

### 2. Potential shortcomings with current pipeline

1. While taking the average slope of all left and right lines, there could
be outliers which can throw the value off. For example, a case of infinite or
zero slope could make the average go to infinite or too low.
2. Taking average slope for left and right lanes for every frame and plotting it
is making these lanes fluctuate a lot which is very visible in output video
3. The image dimensions is adjusted for the available images which has a particular
orientation of camera. Change in camera orientation would require a change
in dimensions too.


---


### 3. Possible improvements to pipeline

1. Outliers of slope can be removed by making sure that slope calculated and 
plotted is within 1-2 standard deviations apart from the mean slope value
2. An averaging of slope from each frame over time instead of using slope for every
frame can bring down the fluctuations. 
3. A parameter setting to change dimension according to camera orientation
can be provided for better performance across vehicles.
