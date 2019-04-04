## About

Full desktop version of ROS indigo. This image also includes a full build
of openCV and some additional ROS tools.

## Installation

ROS indigo is built on top of Ubuntu 14.04, to obtain the base image

``` 
docker pull ubuntu:trusty 
```

Building this image is a bit more complecated due to preparing the image
for allowing to forward the X11 socket.

To build the image navigate to the folder where the Dockerfile is and then
build the image with the following command:

```
docker build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) --build-arg USERNAME=${USER} -t ros-indigo:latest .
``` 
