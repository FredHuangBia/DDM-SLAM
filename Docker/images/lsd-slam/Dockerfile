#
# LSD-SLAM image
#

FROM ros-indigo

MAINTAINER Máté Bartus <mate.bartus@gmail.com>

#
# Install dependencies
#
RUN sudo apt-get update
RUN sudo apt-get install -y \
		ros-indigo-libg2o ros-indigo-cv-bridge liblapack-dev \
		libblas-dev freeglut3-dev libqglviewer-dev \
		libsuitesparse-dev libx11-dev

#
# Install LSD-SLAM
#
RUN cd ~ && mkdir rosbuild_ws && cd rosbuild_ws && \
	rosws init . /opt/ros/indigo && \
	mkdir package_dir && \
	rosws set -y ~/rosbuild_ws/package_dir -t . && \
	echo 'source ~/rosbuild_ws/setup.bash' >> ~/.bashrc && \
	cd package_dir && \
	git clone  https://github.com/rossbar/lsd_slam.git lsd_slam
	
RUN cd ~/rosbuild_ws/package_dir && \
	sed -i "/^#.*openFabMap/s/^#//" ./lsd_slam/lsd_slam_core/CMakeLists.txt && \
	sed -i "/^#.*FABMAP/s/^#//" ./lsd_slam/lsd_slam_core/CMakeLists.txt

RUN /bin/bash -c " \
	sudo rosdep init && rosdep update && \
	source ~/rosbuild_ws/setup.bash && \
	cd ~/rosbuild_ws && \
	rosmake lsd_slam"
	

RUN sudo rm -rf /var/lib/apt/lists/*
