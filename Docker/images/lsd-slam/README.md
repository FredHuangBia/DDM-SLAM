## About

Docker image for LSD-SLAM.

## Installation

The LSD-SLAM image is built from the ROS indigo image, you need to build
that first.

To build the image navigate to the folder where the Dockerfile is located
and then build the image with the following command:

```
docker build -t lsd-slam:latest .
``` 

## Usage

Start the container:

```
docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix lsd-slam /bin/bash
```

When the container is started, you can test it with:

```
roscore > /dev/null &
rosrun lsd_slam_viewer viewer
```

If the viewer window pops up, then everything is working, you can quit the
process by closing the window, then typeing

```
exit
```
