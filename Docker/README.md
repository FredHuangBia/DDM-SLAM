## About

Repository containing docker image files for ROS algorithms.

## Installation

First, you need to install [Docker](https://docker.com).

## Images in the repository

For the individual installation information please see the readme files
for the docker images.

* [ROS indigo](images/ros-indigo/README.md)
* [LSD-SLAM](images/lsd-slam/README.md)

## Notice
- This is forked from [repo](https://github.com/CHItA/ros-docker)
- Two modification were made:
  - The OpenCV is compiled with WITH_GTK flag on and WITH_QT flag off. Otherwise the depth map are not shown correctly
  - The original lsd-slam repo won't compile. The repo download here is based on [repo](https://github.com/rossbar/lsd_slam/)
- The room example is tested to be working on Ubuntu 16.04 docker version 18.09.2

## Steps
First install docker.
Clone this repo:
````
git clone https://github.com/dknyxh/ros-docker
````
Then change directory to ROS indigo image and build that docker file using:
````
cd ros-docker/images/ros-indigo
sudo docker build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) --build-arg USERNAME=${USER} -t ros-indigo:latest .
````
Then build the lsd slam docker image based on this ros-docker by doing:
````
cd ros-docker/images/lsd-slam
sudo docker build -t lsd-slam:latest .
````


To test the lsd slam, launch the docker image first:
````
sudo docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix lsd-slam /bin/bash
````
Then download the test room example inside the docker
````
cd ~
curl http://vmcremers8.informatik.tu-muenchen.de/lsd/LSD_room.bag.zip --output  LSD_room.bag.zip 
unzip LSD_room.bag.zip 
````
Finally, run the roscore and lsd viewer
````
roscore > /dev/null & rosrun lsd_slam_viewer viewer & bg
rosrun lsd_slam_core live_slam image:=/image_raw camera_info:=/camera_info & rosbag play ~/LSD_room.bag
````

