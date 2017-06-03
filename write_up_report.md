# **Finding Lane Lines on the Road**

The primary goal of the project is to make a pipeline that finds lane lines on the image of the road and later use the same technique in a video stream(really just a series of images).


### 1.Reflection

The pipeline is applied to every image and also applied to every frame in the video i.e a series of image. The pipeline was tested with two example videos "solidWhiteRight.mp4" and "solidYellowLeft.mp4".The output of the example videos are "solidWhiteRight_output.mp4" "solidYellowLeft_output.mp4" respectively. 

The pipeline consists of **6** steps.

#### Step 1
The primary step is to read in the image and convert into to grayscale image. The output is an image with single channel color.

`gray = grayscale(image)`

![](test_images_output/SolidWhiteCurve.JPG?raw=true "Original_image")

![](test_images_output/SolidWhiteCurve_grayscale.JPG?raw=true "grayscale_output")

#### Step 2
The Gaussian smoothing with a kernel size = 5 is applied to grayscale output image from step 1.

`blur_gray = gaussianblur(gray, kernel_size)`

![](test_images_output/SolidWhiteCurve_gaussian.JPG?raw=true "gaussian_output")

#### Step 3
The Canny edge transformation is applied to the gaussian output image. A low threshold of 50 and high threshold of 150 is being considered to ignore the pixels.

`edges = canny(blur_gray,low_threshold,high_threshold)`

![](test_images_output/SolidWhiteCurve_canny.JPG?raw=true "canny_output")

#### Step 4
A region of interest is defined by a four sided polygon mask. The vertices are calculated from the size of the canny edged output from step 3. The output is a edge detected image masked in the region of interest.

`masked_edges = region_of_interest(edges)`

![](test_images_output/region_of_interest.JPG?raw=true "region_of_interest_output")

#### Step 5
First, a blank image is created to draw lines on. Second, hough Transformation is applied on the masked edge detected image to output an array containing endpoints of detected line segments. The array is differentiated to separate left and right lines using slope. A positive slope indicates (x being directly proporotional to y) left lanes. A negative slope indicateds (x being inversely proportional to y) right lane. An average of left slope and right slope is being considered to extrapolate the line segments within the region of interest identified in step 4.

`line_image = hough_lines(masked_edges,image)` 

![](test_images_output/extrapolate.JPG?raw=true "extrapolated_output")


#### Step 6
The left and right lanes are drawn on the orignal image with extrapolated points indicating the lane lines on the road. 

![](test_images_output/final_output.JPG?raw=true "Final_output")


### 2. Potential shortcomings

One potential shortcoming would be what would happen when the lane lines are curve lines. This would require change in pipeline. The pipeline is defined with certain global parameters which would require changes if there is changes to the condition in which the video is taken. For example - an image taken at night or at bright sunny light might require fine tuning of the parameters.


### 3. Possible improvements

A color space filter would be better to render better results. Also the line segments above the median range of the average slopes were considered while drawing the lines to filter out. A better way to identify the slopes might help with curve lines too. 