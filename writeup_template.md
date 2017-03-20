##Writeup 
**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"
[image7]: ./output_images/test1_undistorted.jpg
[image8]: ./output_images/threshold.png
[image9]: ./output_images/prepers.png
[image10]: ./output_images/pers.png
[image11]: ./output_images/ot.png

## [Rubric Points](https://review.udacity.com/#!/rubrics/571/view)
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  
---

###Camera Calibration

####1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection with `cv2.findChessboardCorners()`.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function to undistorted the image. the undistorted image is saved in `./output_images`. Most of them are corrected undistorted but image no.1 no.4 and no.5 can't find the chessboard.
###Pipeline (single images)

####1. Provide an example of a distortion-corrected image.
To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one .
![alt text][image2]
the distortion-correction code is in the second cell of the notebook.
the undistorted image is in `./output_images/test1_undistorted.jpg` which is look like this:
![alt text][image7]

####2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.
I used a combination of HSL and gradient thresholds to generate a binary image (thresholding steps at cell no.3 and cell no.4 in the ipython notebook).  first I convet image to HSL space and apply a thresholded to S channel. and in cell no.4 I use sobel x and threshold x gradient to get the final thresholded binary image which is look like this:
![alt text][image8]

####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform is in the 5th code cell in the ipython notebook.  
I manually check in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 600, 440      | 275, 0        | 
| 690, 440      | 1100, 0      |
| 275, 700      | 276, 700      |
| 1100, 700     | 1100, 700     |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image9]

the warped image is look like:

![alt text][image10]
####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]
the code is in the 6th and 7th cell of the ipython notebook.

####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

in the 8th cell I calculated the curvature of the lane and the position of the vehicle with respect to center.

####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in cell no.9.  Here is an example of my result on a test image:

![alt text][image11]

---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

