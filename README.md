# ROS_tutorial
just go through the ROS tutorial

### Installing and Configuring the ROS Environment

```printenv | grep ROS``` checks the ROS Environment (environment variables)

```catkin_make```  creates a ROS Workspace -> creates a CMakeLists.txt link in your 'src' folder, additionally, a 'build' and 'devel' folder. Inside the 'devel' folder -> several setup.\*sh files. Sourcing any of these files will overlay this workspace on top of the environment.

```echo $ROS_PACKAGE_PATH``` check the ROS_PACKAGE_PATH environment variable

### Navigating the ROS Filesystem

- **Packages**: Packages are the software organization unit of ROS code. Each package can contain libraries, executables, scripts, or other artifacts.
- **Manifests** (package.xml): A manifest is a description of a package. It serves to define dependencies between packages and to capture meta information about the package like version, maintainer, license, etc... 
- **Filesystem Tools**
    - ```rospack find [package_name]``` returns the path to package. 
    - ```roscd``` allows you to change directory (cd) directly to a package or a stack. 
        - ```pwd``` - Print Working Directory (shell builtin) 
        - ```roscd log``` takes you to the folder where ROS stores log files.
    - ```rosls``` allows you to list directly in a package by name rather than by absolute path. 

see more in the [rosbash](http://wiki.ros.org/rosbash) suite

### Creating & building a ROS Package

For a package to be considered a catkin package it must meet a few requirements:
- The package must contain a catkin compliant package.xml file (provides meta information about the package).
- The package must contain a CMakeLists.txt which uses catkin.
- Each package must have its own folder

1. ```catkin_create_pkg <package_name> [depend1] [depend2] [depend3]``` creates a catkin Package. (e.g. ```catkin_create_pkg beginner_tutorials std_msgs rospy roscpp``` creates a new package called 'beginner_tutorials' which depends on std_msgs, roscpp, and rospy.)

2. Builde a catkin workspace with ```catkin_make``` and source the generated setup file so as to add the workspace to the ROS environment with ```. ~/[workspace_name]/devel/setup.bash```

3. Set the package dependencies
- check the first-order dependencies with ```rospack depends1 [package_name]```
- check the indirect dependencies with i.e. ```rospack depends1 rospy```
- check all the dependencies with ```rospack depends [package_name]```

4. Customize the package.xml

```
<description>(add the description here)</description>
<maintainer email="user@todo.todo(add the email adress)">user(add the maintainer's name)</maintainer>
<license>TODO</license>
```
5. run ```catkin_make``` to build any catkin projects found in the src folder

If source code in a different place, run 
```
catkin_make --source my_src
catkin_make install --source my_src  # (optionally)
```
### Understanding ROS Nodes

- [Nodes](http://wiki.ros.org/Nodes): an executable that uses ROS to communicate with other nodes.
- [Messages](http://wiki.ros.org/Messages): ROS data type used when subscribing or publishing to a topic.
- [Topics](http://wiki.ros.org/Topics): Nodes can publish messages to a topic as well as subscribe to a topic to receive messages.
- [Master](http://wiki.ros.org/Master): Name service for ROS (i.e. helps nodes find each other)
- [rosout](http://wiki.ros.org/rosout): ROS equivalent of stdout/stderr
- [roscore](http://wiki.ros.org/roscore): Master + rosout + parameter server, a collection of nodes and programs that are pre-requisites of a ROS-based system

run ```rosnode list``` to check the currently running ROS nodes

run ```rosnode info [/name of ROS node]``` to see information about a specific node

```rosrun [package_name] [node_name]```: directly run a node within a package (without having to know the package path). 

### Understanding ROS Topics

The nodes communicate with each other over a ROS Topic. One node publishes the key strokes on a topic, while the other subscribes to the same topic to receive the key strokes. run ```rosrun rqt_graph rqt_graph``` to show the currently running nodes and topics.

- ```rostopic echo [topic]``` shows the data published on a topic. 
- ```rostopic list``` returns a list of all topics currently subscribed to and published. 
- ```rostopic type [topic]``` returns the message type of any topic being published. 
- ```rostopic pub [topic] [msg_type] [args]``` publishes data on to a topic currently advertised.   
    e.g. ```rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'```  
    - dash-one(`-1`): causes rostopic to only publish one message then exit
    - double-dash(`--`): tells the option parser that none of the following arguments is an option
- ```rostopic hz [topic]``` reports the rate at which data is published. 
- ```rosrun rqt_plot rqt_plot``` to display a scrolling time plot of the data published on topics.

### Understanding ROS Services and Parameters

1. ```rosservice``` is another way that nodes can communicate with each other -> allow nodes to send a **request** and receive a **response**. 

- ```rosservice list``` print information about active services
- ```rosservice type``` print service type
- ```rosservice call``` call the service with the provided args
- ```rosservice find``` find services by service type
- ```rosservice uri``` print service ROSRPC uri

2. ```rpsparam``` is used to store and manipulate data on the ROS Parameter Server. The Parameter Server can store integers, floats, boolean, dictionaries, and lists. rosparam uses the YAML markup language for syntax.

- ```rosparam list``` list parameter names
- ```rosparam set``` set parameter
- ```rosparam get``` get parameter
    - ```rosparam get /``` show us the contents of the entire Parameter Server. 
- ```rosparam dump``` dump parameters to file (e.g. params.yaml)
- ```rosparam load``` load parameters from file in a new namespace (e.g. ```rosparam load params.yaml copy```)
- ```rosparam delete``` delete parameter

### Using rqt_console and roslaunch

- ```rqt_console``` attaches to ROS's logging framework to display output from nodes. 
- ```rqt_logger_level``` allows us to change the verbosity level (Fatal>ERROR>WARN>INFO>DEBUG) of nodes as they run. 

```roslaunch``` starts nodes as defined in a launch file. ```roslaunch [package] [filename.launch]```

```
<launch> # launch tag -> the file identified as a launch file 

<group ns="[namespace tag] "> # groups with a namespace tag with a node with a name
   <node pkg="[node]" name="[name]" type="[type_name]"/>
</group>

<node pkg="[node]" name="[name]" type="[type]">
   <remap from="input" to="[topic]"/>
   <remap from="output" to="[topic]"/>
</node>
</launch>
```
### Creating a ROS msg and srv
- **msg** filoe: simple text file that describes the fields of a ROS message. used to generate source code for messages in different languages.  
    - **field types**: Header(a timestamp and coordinate frame information) / int8, int16, int32, int64 (plus uint*) / float32, float64 / string / time, duration / other msg files / variable-length array[] and fixed-length array[C] 
- **srv** file: an srv file describes a service. composed of two parts (request & response). 

make sure that the msg files are turned into source code for C++, Python, and other languages (in package.xml)
```
  <build_depend>message_generation</build_depend>
  <exec_depend>message_runtime</exec_depend>
```
  
(in CMakeLists.txt)
``` 
# Add the message_generation dependency to the find_package call
   find_package(catkin REQUIRED COMPONENTS
   roscpp
   rospy
   std_msgs
   message_generation
)
# Make sure you export the message runtime dependency. 
catkin_package(
  ...
  CATKIN_DEPENDS message_runtime ...
  ...)
  
# Make sure that CMake knows when it has to reconfigure the project after you add other .msg files. 
add_message_files(
  FILES
  Message1.msg
  Message2.msg
)  

# Make sure that CMake knows when it has to reconfigure the project after you add other .srv files. 
add_service_files(
  FILES
  Service1.srv
  Service2.srv
)

# Ensure the generate_messages() function is called
generate_messages(
  DEPENDENCIES
  std_msgs
)
```

Common step for msg and srv: ```catkin_make install``` in the catkin workspace
