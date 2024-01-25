# VSLAM
基于Intel Realsense D435i的视觉惯性[SLAM](https://github.com/SilenceOverflow/Awesome-SLAM) \
本项目高度参考[沈阳航空航天大学哨兵导航框架](https://github.com/tup-robomaster/TUP2023-Sentry-Framework/tree/main)（目前已不含视觉分支），感谢他们 !
<hr>

[![humble][humble-badge]][humble]
[![ubuntu22][ubuntu22-badge]][ubuntu22]

<hr>

## 概述
该项目旨在用深度相机进行SLAM建图及导航，各个功能包功能如下: \
`建图算法`:[octomap_mapping](https://github.com/OctoMap/octomap_mapping/tree/ros2) \
`里程计`:[VINS-Fusion-ROS2](https://github.com/zinuok/VINS-Fusion-ROS2) \
`定位\重定位`:VINS-Fusion-ROS2 \
`2.5D栅格地图`：[grid_map-humble](https://github.com/ANYbotics/grid_map/tree/humble) \
`IMU滤波`:[imu_tools-humble](https://github.com/CCNYRoboticsLab/imu_tools/tree/humble) \
`导航及路径规划`:[navigation2-humble](https://github.com/ros-planning/navigation2/tree/humble)

其中docker文件含构建Ubuntu22.04、ROS2：Humble，以及Intel Realsense SDK、ROS2功能包的dockerfile。构建容器后可用于[Robomaster人工智能挑战赛](https://www.robomaster.com/zh-CN/robo/drone?djifrom=nav_drone)的快速开发。


关于Intel Realsense SDK、ROS2功能包的环境构建，有以下两种方法：
- docker容器构建
  - 本地构建镜像
    ```bash
     docker build --no-cache -t {Image_name}:{tag} .
     #例:docker build --nocache -t slam:test .
    ```
    
  - 构建容器
    ```bash
     # 需要挂载设备dev
     docker run --name {container_name} -it {Image_ID} \ -v /dev:/dev
     #例:docker run --name realsense_test -it 065ba1f13207 \ -v /dev:/dev
    ```
  
- 本地环境构建(默认Humble环境，否则参考[ROS2 Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html))
  - 安装最新的 Intel&reg; RealSense&trade; SDK 2.0 \
    参考Intel官方文档[Linux Debian Installation Guide](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md#installing-the-packages) \
    jetson用户参考[Jetson Installation Guide](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation_jetson.md)
  - 安装Intel&reg; RealSense&trade; ROS2 wrapper \
    创建工作空间
    ```bash
     mkdir -p ~/ros2_ws/src
     cd ~/ros2_ws/src/
      ```
  
    克隆项目
      ```bashrc
       git clone https://github.com/IntelRealSense/realsense-ros.git -b ros2-development
       cd ~/ros2_ws
      ```
  
    安装依赖(网络问题请参考[rosdepc](https://zhuanlan.zhihu.com/p/398754989),rosdep后直接＋c即可)
     ```bash
      sudo apt-get install python3-rosdep -y
      sudo rosdep init 
      rosdep update 
      rosdep install -i --from-path src --rosdistro $ROS_DISTRO --skip-keys=librealsense2 -y
     ```

      构建
     ```bash
      colcon build
     ```

    刷新到环境变量
     ```bash
      source /opt/ros/humble/setup.bash
      cd ~/ros2_ws
      . install/local_setup.bash
     ```
  











[humble-badge]: https://img.shields.io/badge/-HUMBLE-orange?style=flat-square&logo=ros
[humble]: https://docs.ros.org/en/humble/index.html
[ubuntu22-badge]: https://img.shields.io/badge/-UBUNTU%2022%2E04-blue?style=flat-square&logo=ubuntu&logoColor=white
[ubuntu22]: https://releases.ubuntu.com/jammy/
