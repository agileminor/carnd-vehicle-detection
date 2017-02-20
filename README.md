
---

**Vehicle Detection Project**


[//]: # (Image References)
[car_example]: ./output_images/car_example.png
[non_car_example]: ./output_images/notcar_example.png
[heat]: ./output_images/heat_map.png
[labels]: ./output_images/labels_map.png
[bbox]: ./output_images/single_frame.png
[hog_car]: ./output_images/hog_example_car.png
[hog_notcar]: ./output_images/hog_example_notcar.png
[labels]: ./output_images/output.png
[video1]: ./project_output.mp4


###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

HOG features are extracted in the extract_features function, in the 2nd cell of the Jupyter notebook.


I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][car_example]
![alt text][non_car_example]

I tried using spatial features, colour features and HOG features, with various colour spaces and parameters, trying to find both a good result on the test set and a reasonably clean output on test images. Ultimately I settled on YCrCb colour featues with 16 bins and HOG features using 9 orientations, 8 pixels per cell and 2 cells per block.

Examples of HOG results on both car and non car image

![alt text][hog_car]
![alt text][hog_notcar]

With the parameters chosen, I loaded all data from the small dataset provided. The data set is skewed towards non car images (2321 non car vs 1196 car images). I experimented with limiting the number of non car images to make a balanced data set, but it did not have a large effect on the results of my classifier, so I ended up using all the images.

All the features were normalized using StandardScaler, split into training and test data and then used to train a linear SVC. Test accuracy was 0.9763

###Sliding Window Search

For the output images, a sliding window approach is used to tile the image. Image sizes used were 32x32, 96x96, 170x170, 224x224. Only a limited field of the input images was looked at, starting at the horizon line. For video, results of multiple frames were kept and the last 20 were included in creating a thresholded heatmap and then labelled image. Those labels are then used to create detected vehicle bounding boxes on the output image.

Below shows examples of a single frame with matching bboxes and an example after applying heatmap and thresholds/labels.

![alt text][bbox]
![alt text][heat]
![alt text][labels]

Here's a [link to my video result](./project_output.mp4)


---

###Discussion

Like a lot of these projects, the major difficulty is not in building a functional pipeline, but in tuning the parameters. In this case, finding a colour space and matching parameters that provided good positives but controllable false positives took significant time. My image pipeline took multiple seconds per image, resulting in a 1+ hour turnaround time to evaluate on the full video. If I needed to tune it further, I would probably add multi-threading to my code to analyze multiple windows at once. I'd put together a simple script to loop through the parameters and saving the output videos to explore more of the solution space, and then run the whole thing on more powerful AWS instance to minimze time.

In addition, I would use info about the perspective used to trim down the search space, i.e. only looking for at larger windows near the car and smaller windows near the horizon.

It would also be good to try out more data for the classified, based on the full GTI datasets, and possible adding a CNN feature set.

Final note - the example code given in the Udacity lectures is used in parts of the project, where useful.
