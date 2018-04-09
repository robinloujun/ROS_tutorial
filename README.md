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

### Navigating the ROS Filesystem

**Packages**: Packages are the software organization unit of ROS code. Each package can contain libraries, executables, scripts, or other artifacts.

**Manifests** (package.xml): A manifest is a description of a package. It serves to define dependencies between packages and to capture meta information about the package like version, maintainer, license, etc... 

#### Filesystem Tools

1. ```rospack find [package_name]```

returns the path to package. 

2. ```roscd``` 

allows you to change directory (cd) directly to a package or a stack. 

```pwd``` - Print Working Directory (shell builtin) 

```roscd log``` takes you to the folder where ROS stores log files.

3. ```rosls``` 

allows you to list directly in a package by name rather than by absolute path. 

see more in the [rosbash](http://wiki.ros.org/rosbash) suite

### Creating a ROS Package

For a package to be considered a catkin package it must meet a few requirements:

* The package must contain a catkin compliant package.xml file (provides meta information about the package).


* The package must contain a CMakeLists.txt which uses catkin.

*    Each package must have its own folder

#### Creating a catkin Package

```
catkin_create_pkg <package_name> [depend1] [depend2] [depend3]
```
For example, ```catkin_create_pkg beginner_tutorials std_msgs rospy roscpp``` creates a new package called 'beginner_tutorials' which depends on std_msgs, roscpp, and rospy.

##### Building a catkin workspace and sourcing the setup file

```catkin_make```

Then source the generated setup file so as to add the workspace to the ROS environment with ```. ~/[workspace_name]/devel/setup.bash```

##### Set the package dependencies

check the first-order dependencies with ```rospack depends1 [package_name]```

check the indirect dependencies with i.e. ```rospack depends1 rospy```

or check all the dependencies with ```rospack depends [package_name]```

##### Customize the package.xml

```
<description>(add the description here)</description>
<maintainer email="user@todo.todo(add the email adress)">user(add the maintainer's name)</maintainer>
<license>TODO</license>
```
#### Building a ROS Package

run ```catkin_make``` to build any catkin projects found in the src folder

If source code in a different place, run 
```
catkin_make --source my_src
catkin_make install --source my_src  # (optionally)
```
### Understanding ROS Nodes

[Nodes](http://wiki.ros.org/Nodes): an executable that uses ROS to communicate with other nodes.

[Messages](http://wiki.ros.org/Messages): ROS data type used when subscribing or publishing to a topic.

[Topics](http://wiki.ros.org/Topics): Nodes can publish messages to a topic as well as subscribe to a topic to receive messages.

[Master](http://wiki.ros.org/Master): Name service for ROS (i.e. helps nodes find each other)

[rosout](http://wiki.ros.org/rosout): ROS equivalent of stdout/stderr

[roscore](http://wiki.ros.org/roscore): Master + rosout + parameter server, a collection of nodes and programs that are pre-requisites of a ROS-based system

run ```rosnode list``` to check the currently running ROS nodes

run ```rosnode info [/name of ROS node]``` to see information about a specific node

```rosrun [package_name] [node_name]```: directly run a node within a package (without having to know the package path). 

### Understanding ROS Topics

The nodes communicate with each other over a ROS Topic. 

