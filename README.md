This is a fork of the Vector repository at https://github.com/StanleyRoboticsInc

Files have been added to mount a UR 5 robot arm on top.

## Installation


## Bringup


foo
```
roslaunch vector_gazebo vector_ur5_simple.launch
```

foo

```
roslaunch vector_navigation_apps 2d_map_nav_demo.launch map_file:=mymap sim:=true
```

foo
```
roslaunch vector_viz view_robot.launch function:=map_nav
```
foo
```
roslaunch vector_ur5 vector_ur5_planning_execution.launch sim:=true
```
