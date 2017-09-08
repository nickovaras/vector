This is a fork of the Vector repository at https://github.com/StanleyRoboticsInc

Files have been added to mount a UR 5 robot arm on top.

## Installation


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
Open a fourth terminal. To bring up the MoveIt components, run the launch file below. With this running, you can add a Motion Planning element to RViz and control the robot arm. Use RTTConnect for planning. You can select any of the three states stored on the SRDF file and move the robot arm to those positions.
```
roslaunch vector_ur5 vector_ur5_planning_execution.launch sim:=true
```
