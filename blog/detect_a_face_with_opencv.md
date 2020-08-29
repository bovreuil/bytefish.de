title: Detect a face with OpenCV
date: 2010/11/21 17:59
tags: opencv, cpp, python
category: computer vision
slug: object_detection
author: Philipp Wagner
summary: Short snippet on how to perform face detection with the OpenCV2 bindings.

*Please Note: This Post refers to OpenCV 2.x!*

The following sample uses the classes ``video``and ``common``, which are helper classes from the ``samples/python2`` folder. 

The cascades used in this sample are located in the ``data`` folder of your OpenCV download, so you'll probably need to adjust the filename parameter ``cascade_fn`` to make this example work:

```python
import cv2
from video import create_capture
from common import clock, draw_str
 
# You probably need to adjust some of these:
video_src = 0
cascade_fn = "haarcascades/haarcascade_frontalface_default.xml"
# Create a new CascadeClassifier from given cascade file:
cascade = cv2.CascadeClassifier(cascade_fn)
cam = create_capture(video_src)

while True:
  ret, img = cam.read()
  # Do a little preprocessing:
  img_copy = cv2.resize(img, (img.shape[1]/2, img.shape[0]/2))
  gray = cv2.cvtColor(img_copy, cv2.COLOR_BGR2GRAY)
  gray = cv2.equalizeHist(gray)
  # Detect the faces (probably research for the options!):
  rects = cascade.detectMultiScale(gray)
  # Make a copy as we don't want to draw on the original image:
  for x, y, width, height in rects:
    cv2.rectangle(img_copy, (x, y), (x+width, y+height), (255,0,0), 2)
  cv2.imshow('facedetect', img_copy)
  if cv2.waitKey(20) == 27:
    break
```