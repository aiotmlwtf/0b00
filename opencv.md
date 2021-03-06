---
title: tutorials
description: on the Jetson Nano
published: 1
date: 2020-03-01T20:17:30.919Z
tags: jetson nano, opencv
---

## contents
- [basic openCV and object tracking](#how-to-track-objects-with-the-camera)
- [Jetson Nano: Hello AI](#jetson-nano-tutorials)

## basic openCV and object tracking

In this tutorial we will work with a framework called openCV for Computer Vision. It allows us to capture images from the camera and display these in a window and do all kinds of operations on them.

### basic opencv camera setup
The basic principle is to capture a frame, do some processing, and display it in a window. Operations can be anything from changing color information, grabbing a Region of Interest, drawing on top of the image important information, track color, motion, etc.

First, we need to setup openCV in a python program like so:
```python
import cv2
print(cv2.__version__)
dispW=640
dispH=480
flip=2
#Uncomment These next Two Line for the Pi Camera that we use
camSet='nvarguscamerasrc !  video/x-raw(memory:NVMM), width=3264, height=2464, format=NV12, framerate=21/1 ! nvvidconv flip-method='+str(flip)+' ! video/x-raw, width='+str(dispW)+', height='+str(dispH)+', format=BGRx ! videoconvert ! video/x-raw, format=BGR ! appsink'
cam=cv2.VideoCapture(camSet)

#Or, if you have a WEB cam, uncomment the next line
#(If it does not work, try setting to '1' instead of '0')
#cam=cv2.VideoCapture(0)
while True:
    ret, frame = cam.read() # grab a video frame
    
    # all magic (processing) happens here!
    
    cv2.imshow('nanoCam',frame) # display the frame 
    cv2.moveWindow('nanoCam',0,0)
    if cv2.waitKey(1)==ord('q'):
        break
cam.release()
cv2.destroyAllWindows()
```
Open this code from your <kbd>~/code</kbd> folder in Visual Studio Code. Right click in the window and select <kbd>Run Python File in Terminal</kbd>. A window should popup showing a live image from the camera.
Pay attention to the section where most of the (your) processing will take place: <kbd># all magic (processing) happens here!</kbd>

> To close the window: <kbd>HIT Q</kbd> on the keyboard, don't exit in any other way or openCV will become corrupted, requiring a reboot of the Jetson Nano.
{.is-warning}

### change camera image to b/w
```python
import cv2
print(cv2.__version__)
dispW=640
dispH=480
flip=2
#Uncomment These next Two Line for Pi Camera
camSet='nvarguscamerasrc !  video/x-raw(memory:NVMM), width=3264, height=2464, format=NV12, framerate=21/1 ! nvvidconv flip-method='+str(flip)+' ! video/x-raw, width='+str(dispW)+', height='+str(dispH)+', format=BGRx ! videoconvert ! video/x-raw, format=BGR ! appsink'
cam= cv2.VideoCapture(camSet)

#Or, if you have a WEB cam, uncomment the next line
#(If it does not work, try setting to '1' instead of '0')
#cam=cv2.VideoCapture(0)
while True:
    ret, frame = cam.read()
    # display cam video
    cv2.imshow('nanoCam',frame)
    cv2.moveWindow('nanoCam',0,0)
    # convert frame to b/w
    gray=cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY)
    # display b/w video in a new window
    cv2.imshow('GRAY',gray)
    cv2.moveWindow('GRAY',700,0)

    if cv2.waitKey(1)==ord('q'):
        break
cam.release()
cv2.destroyAllWindows()
```
### continue in your ~/code folder
- openCV1 - camera basics
- openCV2 - moving or positioning a window
- openCV3 - save video to a file
- openCV4 - draw lines or text on the video
- openCV5 - animate a bouncing box
- openCV6 - track mouse clicks and display a dot coordinate
- openCV7 - animate a dot with trackbars (sliders)
- openCV9 - Region of Interest (RoI) discolor it
- openCV10 - bounce a ROI around
- openCV11 - copy a ROI to a new window
- openCV12 - add and substract images(masks) with bitwise operator
- openCV13 - create masks with threshold
- openCV14 - move a logo around the screen with masking
- openCV15 - learn to work with color channels
- openCV16 - learn about hsv to select a color with masking

### important openCV features
#### coordinates
All images are treated as rows and columns where the upper-left corner is designated (0,0). So rows go from left to right and columns from top to bottom. Try it out in openCV6. Often you have to think how openCV wants to have the order of the arguments. Does it want row, column order or is it perhaps a simple color array? 
#### color scheme
Each pixel in an image has three values; one for red, one for blue and one for green. We are used to this scheme as RGB, but in openCV it is designated <kbd>BGR</kbd>! Don't get confused.

## Jetson Nano tutorials
NVidia has prepared a few cool tutorials on inferencing [here](https://github.com/dusty-nv/jetson-inference/blob/master/docs/imagenet-console-2.md).
On your Nano we have done the installations, so if all is alright, you can use/experiment with the tuts.
Our goal is to be able to train the Nano to do something you want. But first, just play around and see what happens.

>Warning: don't update the base software on the Nano, it can ruin your day.
{.is-warning}

Check out [community examples](https://developer.nvidia.com/embedded/community/jetson-projects) using the Jetson Nano for a variety of projects like gesture recognition, robotics, drones, etc.

To start a camera inference test, type the following in the terminal:

<kbd>cd jetson-inference/build/aarch64/bin</kbd>
<kbd>./imagenet-camera.py</kbd>

