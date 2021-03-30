# Object Detection & Pedestrian Tracing (Jetson Nano)
The Jetson Nano is a GPU-enabled edge computing platform for AI and deep learning applications. It is powered by a 64-bit quad-core ARM-CortexA57 CPU with 4 GB RAM onboard. It has 128 core Nvidia Maxwell GPU dedicated to several AI and deep learning applications making it suitable for prototype development as well as production.

The Jetson Nano developer kit which houses the Nano module, accessories, pinouts, and ports is ready to use out of the box. It runs a customized Ubuntu 18.04 called Linux4Tegra for the Tegra series chips powering the Nvidia Jetson modules.

For the Object Detection & Pedestrian Tracing task in this repo, we are using the RPi camera module v2. It is powered by a Sony IMX219 8-MP sensor. It supports still images as well as high-definition videos with different resolutions including 1080p@30FPS, 720p@60FPS, and VGA90.

---
## Required Components
* Jetson Nano Developer Kit
* Raspberry Pi Camera v2
* Power Source for Developer Kit
* Keyboard, Mouse and Display Monitor with HDMI support

---
## Connecting the Camera to the Jetson Nano
1. Setup the Jetson Nano Developer Kit using instructions in the introductory article.
2. Add the keyboard, mouse and display monitor.
3. Pull the CSI port and insert the camera ribbon cable in the port. Make sure to align the connection leads on the port with those on the ribbon. The connections on the ribbon should face the heat sink.
4. Power up the developer kit.
5. Verify that the camera is correctly configured and recognized.

The JetPack SDK provided by Nvidia already supports the RPi camera with preinstalled drivers and is readily usable as a plug-and-play peripheral taking away the pain of driver installations and compatibility issues.
You cannot use Cheese, the inbuilt GNOME application for recording videos and taking pictures on the Ubuntu OS, to check the camera connection. This is because the RPi camera is not inherently UV4L compatible and is not identified by default Linux applications.

A simple command-line instruction to check for the camera connection is:
```
abhi@jetson_nano:~$ ls -l /dev/video0
Crw-rw----+ 1 root video 81, 0 Jan 2 14:30 /dev/video0
```
If the output of the command is blank, then it suggests that there is no camera attached to the developer kit. The Nvidia Jetson series uses the GStreamer pipeline for handling media applications.

---
## Setting Up OpenCV
Use Python in the terminal to confirm OpenCV installation and its version.
```
abhi@jetson_nano:~$ python
Python 2.7.15+ (default, Oct 7 2019, 17:39:04)
[GCC 7.4.0] on linux2
Type “help”, “copyright”, “credits” or “license”, for more information
>>>import cv2
>>> cv2.__version__
‘3.3.1’
```

---
## Code Description
The code basically uses OpenCV library. As mentioned earlier, the code uses the GStreamer pipeline to create an interface between the camera and the OS. The pipeline is used to create a VideoCapture() object.

Each image frame from the camera live stream is processed and tested for face detection. The code implements an additional step of converting the color image to a grayscale image since the color does not determine the features. This avoids computational overheads and enhances performance. Once confirmed if the image contains a pedestrian, a rectangle is drawn around the boundaries.


