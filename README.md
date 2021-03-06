# Polygonization of Segmentation Networks
 
In this project I am going to polgonize objects in a binary image 

* First by denoising (Denoise function), bilateral filtering (BilateralFilter function) which can keep edges sharp while removing noises and closing (Closing function) which is useful to detect the overall contour of a figure I am going to segment and count the number of of object (ExtractObjects function) in the figure. 
~~~python
img1 =  cv2.imread('./2.png',0)
denoising = Denoise(img1,20)
BFilter = BilateralFilter(denoising, 7,75,75)
Close = Closing(BFilter,4)
blob_labels, number_of_objects = ExtractObjects(Close)
~~~
* Then by applying on the each object of the image from the previous step, bilateral filtering, closing,  thresholds on X, Y (Abs_sobel_thresh function), magnitude (Mag_thresh function) and direction (Dir_threshold function) gradients to combine them into a binaray image(named combined_gradient) by using Combined_thresholds function I am going to transforms point of the objects into Hough Lines system in order to improve and draw better edges of the object.

* After drawing the edge line of the object (line_image), the contours are extracted (Extract andDraw Contours function) to draw a bounding box (drawBBOX) which considers the direction of the shape.

~~~python

final_img = np.zeros_like(Close)

for i in range(len(number_of_objects)):
    objimg = np.zeros_like(Close)
    print(i)
    

    objimg[blob_labels == i] = 1
    BFilter = BilateralFilter(objimg, 7,75,75)
    Close = Closing(BFilter,2)

    gradx = Abs_sobel_thresh(Close,orient='x',thresh=(40,214) ,sobel_kernel=25)
    grady = Abs_sobel_thresh(Close,orient='y',thresh=(40,214) ,sobel_kernel=25)
    mag_binary = Mag_thresh(Close, sobel_kernel=3, mag_thresh=(60, 200))
    dir_binary = Dir_threshold(Close, sobel_kernel=3, thresh=(.45, .90))
    combined_gradient = Combined_thresholds(gradx,grady,mag_binary,dir_binary)
    
    
    
    lines = DrawLine(combined_gradient)
    line_image = Display_lines(np.copy(combined_gradient),lines)
    contours_img, contours = ExtractandDrawContours(line_image.astype(np.uint8))
    img_box, img_box_2 = drawBBOX(contours,contours_img)
    
~~~

 <p align="center">
<img src="./Images/3.PNG" />
<p align="center">
 
**The final Image is img_box_2**


## To find the center of a shape
~~~
#find Contours
image, contours, hierarchy = cv2.findContours(
                                   image = obj, 
                                   mode = cv2.RETR_TREE, 
                                   method = cv2.CHAIN_APPROX_SIMPLE)3
# sort Contours
contours = sorted(contours, key = cv2.contourArea, reverse = True)

# get the outer Contours
c_0 = contours[0]

## find moment
M = cv2.moments(c_0)
print(M.keys())
# The centroid point
cx = int(M['m10'] / M['m00'])
cy = int(M['m01'] / M['m00'])
~~~

 <p align="center">
<img src="./Images/4.PNG" />
<p align="center">
