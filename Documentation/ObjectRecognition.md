## Using the RbpyCam


```python
#!/usr/bin/env python

import rospy
from sensor_msgs.msg import Image
from cv_bridge import CvBridge, CvBridgeError
import cv2
import numpy as np
```
**Capturing an image from the camera**

We gonna create a class for encapsuling the attributes and callback function:

```python
class TakeAShot(object):

 def __init__(self):
        self.image_sub = rospy.Subscriber("/camera/rgb/image_raw", Image, self.camera_callback)
        self.bridge_object = CvBridge()
```
The callback function will recieve the Image object from the camera in a missage and then it will save the Image as a png file in your 

```python
 def camera_callback(self,data):
      try:
           # We select bgr8 because its the OpenCV encoding by default
           cv_image = self.bridge_object.imgmsg_to_cv2(data, desired_encoding="bgr8")
           cv2.imwrite("image.png", cv_image)
      except CvBridgeError as e:
           print(e)        
```     		
The main is as usual:

```python
def main():

    rospy.init_node('take_a_shot', anonymous=True)
    take_a_shot = TakeAShot()
    try:
        rospy.spin()
    except KeyboardInterrupt:
        print("Shutting down")

if __name__ == '__main__':
    main()
```


### Final Exercise

* Load the room map 
* Start the navigation stack. 
* Get the coordinates of all the traffic sign using Rviz.
* Write a Python node with the folowing behaviour:
	* Go to the first sign.
	* When the robot stops:
		* take an image. 
		* recognize the object using image processing by coulour and shape. 
		* go to next sign according the signs:
			* Left: go to next sign on the left
			* Right: go to next sign on the right
			* STOP: Shutdown node.
			* Forbidden: continues to the next sign  	 


### How to send a sequence of goals to ROS NavStack

* [Sending a sequence of Goals to ROS NavStack with Python](https://hotblackrobotics.github.io/en/blog/2018/01/29/seq-goals-py/)

### Basic Tutorials for image analysis using OpenCV
* [Shape Detection Using findContourns and approxPolyDP ](https://www.pyimagesearch.com/2016/02/08/opencv-shape-detection/)
* [Arrow Interpretation using findContourns and approxPolyDP](https://programs.wiki/wiki/use-opencv-to-judge-the-arrow-direction.html)
* [Detecting lines using Canny Edges and Hough Lines](https://docs.opencv.org/3.4/d9/db0/tutorial_hough_lines.html) --> [Example code ](https://github.com/michael-pacheco/opencv-arrow-detection)
* [Color Detection using HSV space](https://www.pyimagesearch.com/2015/09/14/ball-tracking-with-opencv/)
* [HSV color picker (in opencv H value goes from 0 to 179)](https://alloyui.com/examples/color-picker/hsv.html)




