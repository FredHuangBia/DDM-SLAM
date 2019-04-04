#
# Full desktop version of ROS indigo
#
#
# NOTE: build it with the arguments: 
#		--build-arg USER_ID=$(id -u) GROUP_ID=$(id -g) USERNAME=${USER}
#

FROM ubuntu:trusty

MAINTAINER Máté Bartus <mate.bartus@gmail.com>

# Define build arguments
ARG USER_ID
ARG GROUP_ID
ARG USERNAME

# Setup environment
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

RUN sed -i "/^# deb.*multiverse/ s/^# //" /etc/apt/sources.list
RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 421C365BD9FF1F717815A3895523BAEEB01FA116
RUN echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list
RUN echo "deb http://archive.ubuntu.com/ubuntu trusty multiverse" >> /etc/apt/sources.list

# Setup user
RUN mkdir -p /home/${USERNAME} && \
	echo "${USERNAME}:x:${USER_ID}:${GROUP_ID}:${USERNAME},,,:/home/${USERNAME}:/bin/bash" >> /etc/passwd && \
    echo "${USERNAME}:x:${USER_ID}:" >> /etc/group && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${USERNAME} && \
    echo "source /opt/ros/indigo/setup.bash" >> /home/${USERNAME}/.bashrc && \
    chmod 0440 /etc/sudoers.d/${USERNAME} && \
    chown ${USER_ID}:${GROUP_ID} -R /home/${USERNAME}

# Install opencv 2.4.8
RUN apt-get update && apt-get install -y \
	libopencv-dev build-essential checkinstall \
	cmake pkg-config yasm libtiff5-dev libjpeg-dev \
	libjasper-dev libavcodec-dev libavformat-dev \
	libswscale-dev libdc1394-22-dev libxine2-dev \
	libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev \
	libv4l-dev python-dev python-numpy libtbb-dev libqt4-dev \
	libgtk2.0-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev \
	libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev \
	x264 v4l-utils unzip
	
RUN apt-get install -y git
	
RUN cd /home/${USERNAME} && mkdir opencv && cd opencv && \
	git clone https://github.com/Itseez/opencv.git && \
	cd opencv && git checkout tags/2.4.8 && \
	mkdir build && cd build && \
	cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local \
		-D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON \
		-D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON \
		-D BUILD_EXAMPLES=ON -D WITH_QT=OFF -D WITH_GTK=ON -D WITH_OPENGL=ON .. && \
	make -j 4 && \
	make install && \
	sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf' && \
	ldconfig
	
# Install indigo
RUN apt-get install -y \
		ros-indigo-desktop-full \
		python-rosdep \
		python-rosinstall \
		python-vcstools \
	 && rm -rf /var/lib/apt/lists/*
    
USER $USERNAME
ENV HOME /home/${USERNAME}
