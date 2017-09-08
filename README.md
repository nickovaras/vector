This is a fork of the Vector repository at https://github.com/StanleyRoboticsInc

Files have been added to mount a UR 5 robot arm on top.

### Requirements:

Ubuntu Trusty 14.04

ROS Indigo:   http://wiki.ros.org/indigo/Installation/Ubuntu


## Installation

(If something below does not work or is missing something, refer to
https://github.com/StanleyInnovation/vector_v1/wiki/Setup-Instructions)



```
sudo apt-get update
sudo apt-get upgrade
```

Install additional packages:

```
sudo apt-get install ros-indigo-control-toolbox ros-indigo-moveit-full ros-indigo-costmap-2d ros-indigo-move-base ros-indigo-jsk-recognition ros-indigo-controller-manager ros-indigo-gazebo-ros* ros-indigo-hector* ros-indigo-rviz-imu-plugin ros-indigo-robot-pose-ekf ros-indigo-robot-localization ros-indigo-yocs-cmd-vel-mux ros-indigo-joint-* ros-indigo-csm ros-indigo-costmap-converter ros-indigo-teb-local-planner ros-indigo-navigation ros-indigo-slam-gmapping
```
Install NLOpt:
```
sudo apt-get install libnlopt-dev
```

Open up a terminal and create a new file:
```
sudo gedit /opt/ros/indigo/share/hector_pose_estimation/hector_pose_estimation_nodelets.xml
```
Paste into the empty file:
```
<library path="lib/libhector_pose_estimation_nodelet">
  <class name="hector_pose_estimation/PoseEstimationNodelet" type="hector_pose_estimation::PoseEstimationNodelet" base_class_type="nodelet::Nodelet">
  <description>
    This nodelet initializes the pose estimation filter with a generic system model driven by IMU measurements only.
  </description>
  </class>
</library>
```

Create two workspace directories:
```
mkdir ~/ur_ws
mkdir ~/vector_ws
```

First we take care of the UR part. Clone the ROS-Industrial repository for Universal Robots:

```
cd ~/ur_ws
mkdir src
cd src
catkin_init_workspace
git clone https://github.com/ros-industrial/universal_robot.git
cd ..
catkin_make
source devel/setup.bash
```

Now, do the following ON THE SAME TERMINAL. This is important to make the workspaces overlay, i.e., when you source the second workspace, the elements of the first workspace will also be available.

```
cd ~/vector_ws
mkdir src
cd src
catkin_init_workspace
git clone https://github.com/nickovaras/vector.git
cd ..
catkin_make
source devel/setup.bash
```
Test that the workspaces are overlaid by using the tab-autocomplete function, for example 
```
roslaunch ur_ga [press tab]
```
And it should autocomplete to roslaunch ur_gazebo

This is the end of the installation procedure.


## Bringup


Open a new terminal, source your devel/setup.bash. First we bring the Gazebo simulation up. It simulates a very simple environment (world) stored on the file simple.world
```
roslaunch vector_gazebo vector_ur5_simple.launch
```

Open a new terminal and source again if necessary. Next, start the navigation routines. In this case we will use a previously generated map of the world mentioned above

```
roslaunch vector_navigation_apps 2d_map_nav_demo.launch map_file:=mymap sim:=true
```

Open a third terminal (mind sources). Now let's launch rviz to navigate our environment. You should see the robot sitting on the map, but not necessarily located, as it needs to be initialized. Click on the 2d nav pose button and click on the part of the map where you think the robot actually is, relative to the laser scan also shown on screen. The robot will rotate on its spot to find its bearing. You can repeat the procedure until if it doesn't find its bearings the first time. Once located, you can make the robot navigate clicking on the goal button on RViz and then clicking on the map.
```
roslaunch vector_viz view_robot.launch function:=map_nav
```
Open a fourth terminal. To bring up the MoveIt components, run the launch file below. With this running, you can add a Motion Planning display to RViz and control the robot arm. Use RTTConnect for planning. You can select any of the three states stored on the SRDF file and move the robot arm to those positions.
```
roslaunch vector_ur5 vector_ur5_planning_execution.launch sim:=true
```
If you want to control the robot programatically, please see:

https://github.com/nickovaras/Vector_UR5
