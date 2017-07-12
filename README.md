##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/data_visualization.png #Car and noncar images
[image2]: ./output_images/hog_visualization.png  #HOG image
[image3]: ./output_images/sliding_windows.png    #Sliging Windows technique image
[image4]: ./output_images/windows.png   	 #Shows an example from the pipeline

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

I first read examples from 'vehicle' and 'non vehicle' images. Here I attach asome examples from each one. 

![alt text][image1]

I have used "get_hog_features function". This function contains a flag that is activated depending on if you want to get the HOG image or just the characteristics. 

The parameters that I input to the function are: img, orientations, pixels_per_cell, cells_per_block, trasnform_sqrt, visualise, feature_vector.

This is the output from my function:

![alt text][image2]


####2. Explain how you settled on your final choice of HOG parameters.

I tried with some parameters but I ended up using the same as it was explained during the course. The only thing I change, I add up one more orientation
This is my final choice:
orient = 10
pix_per_cell = 8
cell_per_block = 2

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

The color space I have chosen is 'YCrCb'. I divide the features and labels in X and y and then I scale X. 
This data is splitted into train_data and test_data and then I use a linear svc (Support Vector Classification).

I take the images from images_test and the programm makes the predictions for testing it.

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I define a function that is going to take an image, I set start and stop positions in x and y.The windows size and the overlap fractions are both required parameters (both in x and y dimensions)

Here I attach an snippet: 

![alt text][image3]

After that I define a function to draw bounding boxes. 

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

I use a function called 'single_img_features' that extracts HOG and color features from a list of images. Then I pass the features extracted to the 'search window' which searchs which windows fit for comparing it with the classifier. 

I use another function called 'heat_add' that takes positive pixels for the detected cars. 'thres_applied' decides how many boxes have to overlap the pixels for being accounted as hot. 

'labeled_bboxes' takes hot pixels values for converting them into labels and drawing the bounding boxes. 

![alt text][image4]


### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

The 'cars_finding' function extracts the HOG and color features, scales them and then makes predictions. Using multiple scale values allows for more accurate predictions. I have combined scales of 1.0, 1.5 and 2.0 with their own ystart and ystop values to lower the ammount of false-postive search boxes.



###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I think this method of image recognition is not good for being used in vehicle detection because it takes so much time for processing. I think deep learning is more proper for this scenario.

This pipeline struggles with the appeareance of shadows (as in the previous project) what leads to the creation of false positives. I think adding darker images to both: car and noncars, will help the process.  


