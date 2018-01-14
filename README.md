# **Vehicle Detection and Tracking**

## *Vehicle Detection and Tracking Project Goals*

**The goals / steps of this project are the following:**

	1. Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images
	2. Apply a color transform and spatial binning features, as well as histograms of color, to your HOG feature vector.
	3. Normalize your features and randomize a selection for training and testing.
	4. Train a classifier Linear SVM classifier.
	5. Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
	6. Run your pipeline on a video stream.
	7. create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
	8. Estimate a bounding box for vehicles detected.

-------------------------------------------------------------------------------------------------------------------------------
[//]: # (Image References)

[image1]: ./output_images/1.Vehicles_images.png
[image2]: ./output_images/2.Non_Vehicles_images.png
[image3]: ./output_images/3.Hog_Feature.png
[image4]: ./output_images/4.test_image_Histo.png
[image5]: ./output_images/5.test_image_spatial.png
[image6]: ./output_images/6.test_image_hog.png
[image7]: ./output_images/7.Sliding_Window.png
[image8]: ./output_images/8.Search_window.png
[image9]: ./output_images/9.Heatmap.png
[image10]: ./output_images/10.Pipeline_image.png

## *Writeup / README*

### Provide a Writeup / README

* I have provided a README file that includes all the rubric points and how I addressed each one.

-------------------------------------------------------------------------------------------------------------------------------
## *Histogram of Oriented Gradients (HOG)*

### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

* I started by reading in all the **vehicle** and **non-vehicle** images. Here is an example of one of each of the **vehicle** and **non-vehicle** images:

![alt text][image1]

![alt text][image2]

* I then explored different color spaces and different **skimage.hog()** parameters (**orientations**, **pixels_per_cell**, and **cells_per_block**).

* I grabbed random images from **vehicle** images and displayed them to get a feel for what the **skimage.hog()** output looks like.

* Here is an example using the **YUV** color space and HOG parameters of **orientations = 11**, **pixels_per_cell = (16,16)** and **cells_per_block=(2, 2)**

![alt text][image3]

### 2. Explain how you settled on your final choice of HOG parameters.

* I tried various combinations of parameters for orientation, pix_per_cell and cells_per_cell until I reached to the final combination that allowed me to detect vehicles with high accuracy.

* I have also applied a color transform and spatial binning features, as well as histograms of color, to me HOG feature vector which helped me to increase the number of feature and hence helped me in training my model.

* Here are my final numbers for all the feature extraction:

		Color_Space = 'YUV'
		Spatial_Size = (16,16)
		Histo_Bins = 32
		Histo_Bins_Range = (0,256)
		Orientation = 11
		Pixels_Per_Cell = 16
		Cells_Per_Block = 2
		Hog_Channel = "ALL"

* Here are some examples on the used techniques on test images:

1. **Color Histogram:**


![alt text][image4]

2. **Spatial Binning:**



![alt text][image5]

3. **Hog feature:**



![alt text][image6]



### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

* I trained a linear SVM using LinearSVC(C=0.01), I have used the previous combinations methods to extract my features.

* I have normalize the features and randomize a selection for training with **80%** of data and testing with **20%** of data.

* Finally I have achieved an accuracy of **99.4%**.

-------------------------------------------------------------------------------------------------------------------------------
##  * Sliding Window Search*

### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

* I decided to define an area of search where the vehicle would appear and then I applied multi scale windows with different sizes and overlapping 

* I have used the following windows with the following parameters for each:

	1. Window0:
			x_start_stop = [0,1300]
			y_start_stop = [400, 680]
			window0_size = (64, 64)
			overlap0 = (0.85, 0.85)
	2. Window1:
			x_start_stop = [0,1300]
			y_start_stop = [400, 680]
			window1_size = (96, 96)
			overlap1 = (0.75, 0.75)
	3. Window2
			x_start_stop = [0,1300]
			y_start_stop = [400, 680]
			window2_size = (128, 128)
			overlap2 = (0.65, 0.65)

Here's an example of the windows scale:

![alt text][image7]



### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]

-------------------------------------------------------------------------------------------------------------------------------

## *Video Implementation*

### 1. Provide a link to your final video output.

* Here's a [link to my video result](./project_video_output.mp4)

### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps:

![alt text][image5]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



-------------------------------------------------------------------------------------------------------------------------------

## *Discussion*

### Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I will discuss my pipeline, the methods I used to detect and track the vehicle in the video, what are the problems I faced, how I fixed them and what are the improvements that can used.

#### 1. PipeLine:



#### 2. Improvements:



