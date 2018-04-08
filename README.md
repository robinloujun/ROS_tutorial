# ROS_tutorial
just go through the ROS tutorial

check the ROS Environment (environment variables)
```
printenv | grep ROS
```
Create a ROS Workspace
```
cd [workspace_name]
catkin_make
```
[catkin_make](http://wiki.ros.org/catkin/commands/catkin_make) create a CMakeLists.txt link in your 'src' folder, additionally, a 'build' and 'devel' folder.

Inside the 'devel' folder you can see that there are now several setup.*sh files. Sourcing any of these files will overlay this workspace on top of your environment.
