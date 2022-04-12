# build command: docker build --tag ghcr.io/patrick-5546/sailbot_ros2_workspace/base:raye .
FROM ghcr.io/patrick-5546/sailbot_ros2_workspace/base:ros_eloquent-ompl_1_5_0_python3 as builder

###################### ROS1 Setup ######################
# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu bionic main" > /etc/apt/sources.list.d/ros1-latest.list

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

ENV ROS1_DISTRO melodic
ENV ROS2_DISTRO eloquent

# install ros packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-melodic-ros-comm=1.14.11-1* \
    ros-melodic-roscpp-tutorials=0.9.3-1* \
    ros-melodic-rospy-tutorials=0.9.3-1* \
    && rm -rf /var/lib/apt/lists/*

# install ros2 packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-eloquent-ros1-bridge=0.8.3-1* \
    ros-eloquent-demo-nodes-cpp=0.8.4-1* \
    ros-eloquent-demo-nodes-py=0.8.4-1* \
    && rm -rf /var/lib/apt/lists/*

# install downstream packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    python-rosdep \
    && rm -rf /var/lib/apt/lists/*

FROM builder
LABEL org.opencontainers.image.source=https://github.com/patrick-5546/sailbot_ros2_workspace

###################### Raye Software Setup ######################
COPY . /ompl
WORKDIR /ompl
RUN ./ompl_install_instructions.bash

WORKDIR /
