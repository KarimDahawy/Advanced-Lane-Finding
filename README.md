# **Advanced Lane Finding**

## *Advanced Lane Finding Project Goals*

**The goals / steps of this project are the following:**

	1. Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
	2. Apply a distortion correction to raw images.
	3. Use color transforms, gradients, etc., to create a thresholded binary image.
	4. Apply a perspective transform to rectify binary image ("birds-eye view").
	5. Detect lane pixels and fit to find the lane boundary.
	6. Determine the curvature of the lane and vehicle position with respect to center.
	7. Warp the detected lane boundaries back onto the original image.
	8. Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

-------------------------------------------------------------------------------------------------------------------------------
[//]: # (Image References)

[image1]: ./output_images/1.undistorted_calibrated_images.png
[image2]: ./output_images/2.undistorted_test_images.png
[image3]: ./output_images/3.binary_test_images.png
[image4]: ./output_images/4.warped_test_images.png
[image5]: ./output_images/5.histogram_test_images.png
[image6]: ./examples/color_fit_lines.jpg
[image7]: ./output_images/6.draw_lines_test_images.png
[image8]: ./output_images/7.draw_lines_world_test_images.png
[image9]: ./output_images/8.draw_lines_world_data_test_images.png

## *Writeup / README*

### Provide a Writeup / README

* I have provided a README file that includes all the rubric points and how I addressed each one.

-------------------------------------------------------------------------------------------------------------------------------
## *Camera Calibration*

### Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

* The code for this step is contained in  **Get Calibration Coefficients** cell of the IPython notebook **Advanced_Lane_Finding_Project.ipynb**.
The above cell calculations depend on calculating corner points of a chessboard which located in **Get Corner points** cell then calculate camera matrix and distortion coefficients which located in **Camera Calibration and undistorted images** cell.

* I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, **objp** is just a replicated array of coordinates, and **obj_points** will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  **img_points** will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection. 

* I then used the output **obj_points** and **img_points** to compute the camera calibration and distortion coefficients using the **cv2.calibrateCamera()** function.  I applied this distortion correction to the **calibration images** in [**camera_cal**](https://github.com/KarimDahawy/Advanced-Lane-Finding/tree/master/camera_cal)  folder using the **cv2.undistort()** function and obtained this result:


![alt text][image1]


* As noticed from the above image, I couldn't get some images corner points as the size of these images were lower that the expected size **(9x6)**.

* Images names are as follows:

	* calibration_1.jpg
	* calibration_4.jpg
	* calibration_5.jpg

-------------------------------------------------------------------------------------------------------------------------------
## *Pipeline (single images)*

### 1. Provide an example of a distortion-corrected image.

* I have implemented a function called **undistort_image()** and can be found in **Undistorted Image** cell of the IPython notebook **Advanced_Lane_Finding_Project.ipynb**, then I have used it for testing all the test images in **Test Images for Distortion, warping and applying color thresholds** cell.

* Here's my output for this step for the test images in [**test_images**](https://github.com/KarimDahawy/Advanced-Lane-Finding/tree/master/test_images) folder.

![alt text][image2]


### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

* I used a combination of two color methods to generate a binary image. This combination depends on calling function **combined_thresholds()** which can be found in **Combined Color & Gradient Methods** cell.

* Function **combined_thresholds()** depends on calling two different functions which represent the following color methods:
	1. Function **lab_select()** which represents LAB color method through using **B-channel**.
	2. Function **hls_select()** which represents HLS color method through using **L-channel**.

* I have used function **combined_thresholds()** for testing all the test images in **Test Images for Distortion, warping and applying color thresholds** cell.

* Here's my output for this step for the test images in [**test_images**](https://github.com/KarimDahawy/Advanced-Lane-Finding/tree/master/test_images) folder.

![alt text][image3]


### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

* The code for my perspective transform includes a function called **warper()**, which located in **Perspective Transform** cell of the IPython notebook **Advanced_Lane_Finding_Project.ipynb**.  The **warper()** function takes as inputs an image **(Image)**, as well as source **(SourcePoints)** and destination **(DestinationPoints)** points.  I chose the source and destination points in the following manner:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 575, 460      | 250, 0        | 
| 720, 460      | 1050, 0      |
| 100, 720     | 250, 700      |
| 1200, 720      | 1050, 700        |

* I verified that my perspective transform was working as expected by drawing the **(SourcePoints)** and **(DestinationPoints)**  points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

* I have used function **warper()** for testing all the test images in **Test Images for Distortion, warping and applying color thresholds** cell.

* Here's my output for this step for the test images in [**test_images**](https://github.com/KarimDahawy/Advanced-Lane-Finding/tree/master/test_images) folder.

![alt text][image4]


### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

* First I have calculated the histogram for all the warped test images to make sure that the detected lines have the highest peaks.

* I have used function **calc_histogram()** which located in **Calculate Histogram** cell for testing all the test images in **Draw Histogram of images in test_images folder** cell.

* Here's my output for this step for the test images in [**test_images**](https://github.com/KarimDahawy/Advanced-Lane-Finding/tree/master/test_images) folder.

![alt text][image5]

* Then I used the histogram data to fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image6]

* I have used function **Visualize_Lines()** which located in **Find Lines by Implement Sliding Windows and Fit a Polynomial** cell for testing all the test images in **Draw Lines, curvature radius and Vehicle position on test images** cell.

* Here's my output for this step for the test images in [**test_images**](https://github.com/KarimDahawy/Advanced-Lane-Finding/tree/master/test_images) folder.

![alt text][image7]


### 5. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

* I have used function **draw_lines()** which located in **Draw Lines on Original Images** cell for testing all the test images in **Draw Lines, curvature radius and Vehicle position on test images** cell.

* Here's my output for this step for the test images in [**test_images**](https://github.com/KarimDahawy/Advanced-Lane-Finding/tree/master/test_images) folder.

![alt text][image8]


### 6. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

* I have used function **Get_Curvature()** which located in **Get Lines Radius** cell to get the lines radius ,then I used function **plot_data** to plot the right and left lines radius and calculate vehicle position for testing all the test images in **Draw Lines, curvature radius and Vehicle position on test images** cell.

* Here's my output for this step for the test images in [**test_images**](https://github.com/KarimDahawy/Advanced-Lane-Finding/tree/master/test_images) folder.

![alt text][image9]


-------------------------------------------------------------------------------------------------------------------------------
## *Pipeline (video)*

### Provide a link to your final video output. 

Here's a [**link to my video result**](./project_video_output.mp4)

-------------------------------------------------------------------------------------------------------------------------------
## *Discussion*

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?
