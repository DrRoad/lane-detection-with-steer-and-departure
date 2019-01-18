# Lane Detection with Steer Assist and Lane Departure Monitoring

## Project Description & Objective
This project was created to demonstrate how a lane detection system works on cars equipped with a front facing camera.
Finding a place in more and more vehicles, this system is an essential part of the advanced driver assistance systems (ADAS) used in
autonomous / semi-autonomous vehicles. This feature is responsible for detecting lanes, measuring curve radius (tightness of a curve) and 
monitors if the vehicle is drifting away from its current lane and warns the driver to prevent unwanted traffic incidents. With this 
information, this system significantly improves safety by making sure the vehicle is centered inside the lane lines, as well as adds comfort if it is also configured to control steering wheel to take gentle curves on highways without any driver input. This version here is a simplified version and best functions if good conditions are provided (clear lane lines, stable light conditions). In this repository, it is included a video of a dash cam for the script to work with.

#### [**Demo Video**](https://youtu.be/R9Ee8Zcqax0) (Click to see the full video)

![](https://imgur.com/CujBKoY.gif)

---

## Get This Project Up and Running

This project follows a simplistic approach in terms of how to run it. Only 2 files are needed in order to get it running.
* [laneDetection](laneDetection) is the only file to perform image processing nad detecting lanes
* [drive](drive) is the video file on which image processing is performed.

Simply clone this repository to desired path by launching command prompt and running
```git clone https://github.com/canozcivelek/lane-detection-with-steer-and-departure.git```
Make sure both files are in the same directory.

#### Prerequisites
To successfully run the project, it is required to have the following software and their respective versions installed:
* Python 3.6 or higher (https://www.python.org/downloads/)
* OpenCV 3 or higher (Will be used by the script and can be downloaded using pip)
* Numpy 1.14 or higher (Will be used by the script and can be downloaded using pip)
* Scipy 1.1 or higher (Will be used by the script and can be downloaded using pip)

#### Dependencies and Environment
It should be noted that this project was developed on a Microsoft Windows 10 (64 Bit) machine, however, it should work on other platforms too with minor tweaks here and there. As for Python packages, follow the instructions on the previous section (Prerequisites) to have the correct versions of dependecies.


## How the Code Works 
A video file contatining dashcam footage of a car cruising along the highway is provided for the script ```laneDetection.py```
Following a modular approach, the Python script has several functions to perform lane detection.

#### readVideo()
First up is the readVideo() function to access the video file ```drive.mp4``` which is located in the same directory.

#### processImage()
This function performs some processing techniques to isolate white lane lines and prepare it to be further analyzed by the upcoming functions. Basically, it applies HLS color filtering to filter out whites in the frame, then converts it to grayscale which then 
is applied thresholding to get rid of unnecessary detections other than lanes, gets blurred and finally edges are extracted with cv2.Canny() function.

#### perspectiveWarp()
Now that we have the image we want, a perspective warp is applied. Simply put, 4 points are put on the frame such that they surround only the area which lanes are present, then maps it onto another matrix to create a birdseye look at the lanes. This will enable us to work with a much refined image and help detecting lane curvatures.

#### plotHistogram()
Plotting a for the bottom half of the image is an essential part to obtain the information of where exactly the left and right lanes start. Upon analyzing the histogram, one can see there is two distinct peaks where all the white pixels are detected. A very good indicator of where the left and right lanes begin. Since the histogram x coordinate represent the x coordinate of our analyzed frame, it means we now have x coordinates to start searching for the lanes.

#### slide_window_search()
A sliding window approach is used to detect lanes and their curvature. It uses information from previous histogram function and puts a box with lane at the center. Then puts another box on top based on the positions of white pixels from the previous box and places itself accordingly all the way to the top of the frame. This way, we have the information to play around with and make some calculations. Then, a second degree polynomial fit is performed to have a curve fit in pixel space.

#### general_search()
After running the slide_window_search() function, this general_search() function is now able to fill up an area around those detected lanes, again applies the second degree polyfit to the draw a yellow line which overlaps the lanes pretty well. This line will be used to measure radius of curvature which is essential in predicting steering angles.











