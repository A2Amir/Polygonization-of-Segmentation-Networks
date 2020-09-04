# Polygonization
 
In this project I am going to polgonize objects in a binary image 

First by denoising (Denoise function), bilateral filtering (BilateralFilter function) which can keep edges sharp while removing noises and closing (Closing function) which is useful to detect the overall contour of a figure I am going to segment the number of of object (ExtractObjects function) in the figure. 

Then by applying on the each object of the image from the previous step, thresholds on X, Y (Abs_sobel_thresh function), magnitude (Mag_thresh function) and direction (Dir_threshold function) gradients to combine them into a binaray image(named combined_gradient) by using Combined_thresholds function I am going to transforms point into Hough Lines system in order to better draw edges of the object.



Then I combine 
