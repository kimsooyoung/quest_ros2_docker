# Dockerfile with Installation Instructions
FROM osrf/ros:noetic-desktop-full
ARG USER=user
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    git \
    vim \
    libatlas-base-dev \
    libeigen3-dev \
    libgoogle-glog-dev \
    libsuitesparse-dev \
    python3-catkin-tools \
    python3-osrf-pycommon \
    python3-matplotlib \
    gfortran \
    autoconf \
    coinor-libipopt-dev \
    curl \
    libopenmpi-dev \
    apt-utils \
    software-properties-common \
    build-essential \
    libssl-dev && rm -rf /var/lib/apt/lists/*
RUN rosdep update

RUN apt-get update && apt-get install -y \
    ros-noetic-actionlib-tools \
    python3-pip && rm -rf /var/lib/apt/lists/*

# ROS 2 Install
RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
RUN sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
RUN apt update

# for ros1 apt-install
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
RUN apt update

RUN apt install -y ros-foxy-desktop
RUN apt install -y python3-pip
RUN apt install -y ros-foxy-ros1-bridge

RUN pip install flask flask-ask-sdk ask-sdk
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash"

# # This one required for catkin build
# RUN rm -rf /home/${USER}/.catkin_tools
# RUN catkin config --extend /opt/ros/noetic
# RUN rm -rf /.catkin_tools

RUN mkdir -p /home/${USER}/quest2ros_ws/src
WORKDIR /home/${USER}/quest2ros_ws/src
RUN git clone https://github.com/Unity-Technologies/ROS-TCP-Endpoint.git \
    && git clone https://github.com/Road-Balance/quest2ros.git

WORKDIR /home/${USER}/quest2ros_ws
RUN cd /home/${USER}/quest2ros_ws && catkin init 

