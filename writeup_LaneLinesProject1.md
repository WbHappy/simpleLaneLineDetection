# **Finding Lane Lines on the Road** 

## Pipeline description

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The developped pipeline consisted of the five different steps that were introduced in the previous lessons:
* Grayscaling the image, using OpenCV.
* Apply a gaussian smoothing on the image, using OpenCV.
* Apply the Canny algorithm to get all wanted edges, using OpenCV.
* Apply a trapezoidal region of interest to only focus on the part of the image that we want.
* Using Hough Transform, again using OpenCV, we can find part of left and right lines.
* Finally, we just need to add left line image and right line image to original image.

Take a look at **folder find_line_output_version_1** to see image results of this.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function:
* Dividing left and right segment by computing slopes.
	* Left line should have a positive slope
	* Right line should have a negative slope
	* Storing coefficients of segments.
* Look for mean left and right line coefficients (m,b) using a polynomial fit provided by numpy.
* Use two Y known coordinates : image_size and Y of our region of interest to compute complete line using the previous (m,b) coeffcients.

Take a look at **folder find_line_output_version_2** to see image results of this.


### 2. Identify potential shortcomings with your current pipeline


I can see four fatal flaws for my pipeline :
* The way the Region of interest of computed is too manual. It should be a percentage of the image and not a static pixel value. 
* If the points on a line are too far from each other, the line won't be computed correctly.
* I am looking for a specific range of slopes, appart from checking the zero slope I should probably leave the rest but in this case that will leave too many false positives.
* Likely the use of a simple RGB camera will be a flaw in itself. During the night the pipeline will not work, but also with strong shadows, rain, fog or snow. There is a need to fuse with the output of other type of sensors.


### 3. Suggest possible improvements to your pipeline

An obvious improvement would be to have a region of interest that is directly function of the image.

From time to time the distance between white marks on a line changes. The hough parameters are static which won't be compatible with this case: an improvement would be to detect this and automatically change the parameters. 