FROM ros:humble-ros-base

ARG DEBIAN_FRONTEND=noninteractive
#Register the server's public key
RUN apt-get update && \
    apt-get upgrade -y && \
    apt install curl -y && \
    mkdir -p /etc/apt/keyrings && \
    curl -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo tee /etc/apt/keyrings/librealsense.pgp > /dev/null

#Make sure apt HTTPS support is installed,and add the server to the list of repositories
RUN echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo `lsb_release -cs` main" | \
    tee /etc/apt/sources.list.d/librealsense.list && \
    apt-get update && \
    apt-get upgrade -y

#Install the libraries and Optionally install the developer and debug packages
RUN apt-get install librealsense2-dkms -y && \
    apt-get install librealsense2-utils -y && \
    apt-get install librealsense2-dev -y && \
    apt-get install librealsense2-dbg -y

#Install Intel® RealSense™ ROS2 wrapper
#1.create workspace
RUN mkdir -p /ros_ws/src
WORKDIR /ros_ws/

#2.clone projects
RUN cd src && git clone https://github.com/IntelRealSense/realsense-ros.git -b ros2-development --depth=1

#3.install dependencies and some tools
RUN apt-get install python3-rosdep -y && \
    rm -rf /etc/ros/rosdep/sources.list.d/20-default.list && \
    rosdep init && \
    rosdep update && \
    rosdep install -i --from-path src --rosdistro $ROS_DISTRO --skip-keys=librealsense2 -y

#4.build
RUN . /opt/ros/humble/setup.sh && colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release

#5.Source environment
SHELL ["/bin/bash", "-c"]
RUN ROS_DISTRO=humble && \
    source /opt/ros/$ROS_DISTRO/setup.bash && \
    . install/local_setup.bash