# pplanner_simulator — Dependencies and Installation Guide

This document enumerates the external packages commonly required to build and run this project (ROS Noetic) and provides reproducible installation steps and troubleshooting guidance.

## Target Environment
- OS: Ubuntu 20.04 (or compatible)
- ROS distribution: Noetic

Ensure ROS repositories are configured and the system is up to date:
```bash
sudo apt-get update
sudo apt-get upgrade -y
```

## One-command Installation (Recommended)
Install the commonly used dependencies:
```bash
sudo apt-get install -y \
  ros-noetic-joy \
  ros-noetic-octomap-msgs \
  libignition-math4-dev \
  liblcm-dev \
  ros-noetic-twist-mux \
  ros-noetic-interactive-marker-twist-server \
  ros-noetic-move-base-msgs \
  ros-noetic-mavlink \
  ros-noetic-octomap-ros \
  libgoogle-glog-dev
```

## Dependency Overview and Purpose
- ros-noetic-joy: Joystick input support (joy node).
- ros-noetic-octomap-msgs: OctoMap message definitions.
- libignition-math4-dev: Ignition Math library (provides ignition/math.hh).
- liblcm-dev: LCM communication library (links with -llcm).
- ros-noetic-twist-mux: Velocity command multiplexer.
- ros-noetic-interactive-marker-twist-server: Twist server via interactive markers.
- ros-noetic-move-base-msgs: Message definitions for move_base.
- ros-noetic-mavlink: MAVLink messages packaged for ROS.
- ros-noetic-octomap-ros: ROS integration for OctoMap.
- libgoogle-glog-dev: Google glog logging headers and libraries (glog/logging.h).

## Common Build Errors and Fixes
- cannot find -llcm
  - Cause: Missing LCM development package.
  - Fix: `sudo apt-get install liblcm-dev`
- Failed to find glog - Could not find glog include directory, set GLOG_INCLUDE_DIR to directory containing glog/logging.h
  - Cause: Missing glog headers.
  - Fix: `sudo apt-get install libgoogle-glog-dev`
- fatal error: ignition/math4/ignition/math.hh: No such file or directory
  - Cause: Missing Ignition Math v4.
  - Fix: `sudo apt-get install libignition-math4-dev`
- Missing octomap_msgs / octomap_ros / move_base_msgs (cannot find headers or catkin packages)
  - Cause: Corresponding ROS message packages not installed.
  - Fix: Install the corresponding ros-noetic-xxx packages (see the one-command installation above).

## Post-install Verification
- ROS packages visible:
  ```bash
  rospack find octomap_ros
  rospack find move_base_msgs
  ```
- Libraries/headers present:
  ```bash
  ldconfig -p | grep lcm
  dpkg -L libgoogle-glog-dev | grep logging.h
  dpkg -L libignition-math4-dev | grep ignition/math.hh
  ```

## Build Instructions
- For a catkin workspace (example):
  ```bash
  cd ~/planner_simulator_ws
  catkin build -DCMAKE_BUILD_TYPE=Release
  source devel/setup.bash
  ```
If the build fails due to missing dependencies, install the relevant packages as described above and rebuild.

