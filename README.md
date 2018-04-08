# ROS_tutorial
just go through the ROS tutorial

### Installing and Configuring the ROS Environment
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

check the ROS_PACKAGE_PATH environment variable
```
echo $ROS_PACKAGE_PATH
```

# Navigating the ROS Filesystem

**Packages**: Packages are the software organization unit of ROS code. Each package can contain libraries, executables, scripts, or other artifacts.

**Manifests** (package.xml): A manifest is a description of a package. It serves to define dependencies between packages and to capture meta information about the package like version, maintainer, license, etc... 

### Filesystem Tools

1. ```rospack find [package_name]```

returns the path to package. 

2. ```roscd``` 

allows you to change directory (cd) directly to a package or a stack. 

```pwd``` - Print Working Directory (shell builtin) 

```roscd log``` takes you to the folder where ROS stores log files.

3. ```rosls``` 

allows you to list directly in a package by name rather than by absolute path. 

see more in the [rosbash](http://wiki.ros.org/rosbash) suite
