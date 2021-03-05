<h2>Multi Class Multi Object Random Moving Detection and Tracking<h2>
<h5>By: Sanyam Sharma, Sarthak Pal, Utsav Goyal<h5>

<h3>AIM<h3>
 
The aim of the project is to utilize Deep Learning techniques for multi-class multi-object classification in order to track the trajectories of the fishes in 3D space using MS Kinect camera which provides depth frames alongside RGB frames.


<h3>Objective<h3>
 
We aim to distinguish the pattern of different species as well as different fishes of same species each having their own unique ID. We'll be using Deep Learning techniques to detect fishes and MS KinectV2 to record data in a controlled environment.

<b>Data Gathering<b>
 
Collecting raw primary data using Kinect V2
The Microsoft Kinect sensor is a peripheral device (designed for Xbox and windows PCs) that functions much like a webcam. However, in addition to providing an RGB image, it also provides a depth map. 
Meaning for every pixel seen by the sensor, the Kinect measures distance from the sensor. The Kinect V2, is a 3D sensor produced by Microsoft, it is composed by a RGB camera with resolution of 1920×1080 pixels, a depth camera with resolution of 534×451 pixels and an infrared emitter with a time stamp. 

The dataset used was gathered using KinectV2 in a control environment. Data was recorded and images on RGB frames, depth frames and infrared frames were extracted from the video for different values of concurrent timestamps.

<b>Preparing Data<b>
 
Upon gathering data, we are left with RGB data frames, depth frames and infrared frames of all the respective pictures we extracted from the video. Now the next step comes is to align these depths and RGB images with infrared to analyze 3D view and utilize temperature information. Appropriate images were selected using the same timestamp and were mapped together using intrinsic values of Kinect V2.


<b>TimeStamp<b>
 
The timestamps of depth image and color image of a frame were not same so to synchronize those we observed the patterns and mapped those images together which had the minimum absolute difference in their timestamp

Mapping RBG frame to Depth frame:
While the Kinect’s color and depth streams are represented as arrays of information, you simply cannot compare the x & y coordinates between the sets of data equally

To map RGB with depth, we have to calibrate our Kinect V2 using check board configuration to find intrinsic values of Kinect. Each and every Kinect has a slightly different set of intrinsic values. These intrinsic values along with the rotational matrix is used to map the same.

<b>Mapping depth pixels with color pixels<b>
 
The first step is to undistort RGB and depth images using the estimated distortion coefficients. Then, using the depth camera Intrinsics, each pixel (x_d,y_d) of the depth camera can be projected to metric 3D space using the following formula:

P3D.x = (x_d - cx_d) * depth(x_d,y_d) / fx_d

P3D.y = (y_d - cy_d) * depth(x_d,y_d) / fy_d

P3D.z = depth(x_d,y_d)

with fx_d, fy_d, cx_d and cy_d the intrinsics of the depth camera.
We can then reproject each 3D point on the color image and get its color:

P3D' = R.P3D + T

P2D_rgb.x = (P3D'.x * fx_rgb / P3D'.z) + cx_rgb

P2D_rgb.y = (P3D'.y * fy_rgb / P3D'.z) + cy_rgb

with R and T the rotation and translation parameters estimated during the stereo calibration.


<b>YOLOv5s<b>
 
We did the necessary augmentation so that it could learn better. We used the small version of YOLOv5 in order to get higher FPS from the system because the problem statement requires a real-time system. We achieved inference of 0.01 seconds on our test data (~100 FPS for our system).
This image consists of all the fishes detected with correct classes assigned which is depicted by the first numbers written in the bounding box with high confidence values written next to the class ID number in the bounding boxes. 


<b> Generating csv<b>
 
 After generating all the bounding boxes, we used the middle point of bounding box as the coordinate of fish. After retracing the middle point of bounding box through all the resizing and cropping steps, we used the RGB and D mapping we created earlier to mark the coordinates of the fish, and hence the system was complete.

