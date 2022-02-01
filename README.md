# ROS_Topics_Services
Basics of setting up ROS Workspace, creating packages and custom ROS topics, messages and services.

# Linux ROS commands:

```
Environment Setup:
gedit ~/.bashrc  (open the bashrc file in the home directory(.bashrc is a file containing several script commands which is automatically executed when you run a terminal)
source /opt/ros/noetic/setup.bash (You must add this setup.bash script in bashrc file to enable the default ros workspace)
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc(automatically source above script in the bashrc file)
source ~/.bashrc

#Set up  your own Ros Workspace: (your catkin workspace will be used to create and store your own ROS packages (project))
#catkin is the name of the build tool used to compile and execute programs in ROS
mkdir -p catkin_ws/src (create src folder to store the ros packages)
cd ~/catkin_ws/src 
catkin_init_workspace
cd ~/catkin_ws/ 
catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc (adding workspace environment to bashrc file automatically)
source ~/.bashrc
roscd( it is used to access the default catkin workspace created)

Create ros packages inside src file:
cd~/catkin_ws/src
catkin_create_pkg ros__tutorials std_msgs rospy roscpp(dependencies libraries)
cd ..(come out of src folder)
catkin_make   (to compile/build the packages inside workspace(this will generate development,configuration files and source files(contain the source files) for the project))

ROS Ecosystem:
$roscore- master node
$rosnode list  (to see  the no of nodes)
$rostopic list (topics are messages that can be published or subscribed by other nodes)

$rosrun(used to run the ros node) turtlesim(package name) turtlesim_node(publisher node name)
$rosnode info /turtlesim (find information of the node)
$rostopic info /turtle1/cmd_vel (displays message related to topic cmd_vel, a ros message is the entity exchanged between two nodes to convey information about the topic)
$rosmsg show geometry_msgs/Twist (shows the content of ROS message)

$rosrun turtlesim turtle_teleop_key (a node that publishes cmd_vel topic when we press keyboard arrows to move the robot)
$rostopic echo /turtle1/cmd_vel ( to see linear and angular velocity values)

$rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'

rqt graphs :
$rosrun rqtgraph rqtgraph

start master node(roscore)
Add executables in the cmakelist file inside package folder of ros__tutorials
#talker
add_executable(talker src/talker.cpp)
target_link_libraries (talker ${catkin_LIBRARIES})

#listener
add_executable(listener src/listener.cpp)
target_link_libraries (listener ${catkin_LIBRARIES})

Compile ros cpp and py files using catkin_make:

rostrum ros__tutorials talker
rosrun ros__tutorials talker

Steps to create new ROS Message:
create a message folder in your package
create the message file with .msg extension
edit the message file by adding elements
Update the dependencies:
in package.xml ( <build_depend>message_generation</build_depend> , <exec_depend>message_runtime</exec_depend>)
in CmakeLists.txt ## Generate messages in the 'msg' folder
 add_message_files(
   FILES
   IoTSensor.msg
 )
compile the package using catkin_make
make sure your message is created with rosmsg show <msgfilename>

To publish custom iot_sensor_topic:
$ rosrun ros__tutorials iot_sensor_publisher.py
$ rostopic echo iot_sensor_topic (to listen to the iot_sensor topic)

ROS Services:

$rosservice list
$rosservice info /spawn
$rossrv info turtlesim/Spawn (to display the structure of the message type of service spawn)
$osservice call /spawn 7 7 180 t1

$rossrv list
$rossrv show ros__tutorials /AddTwoInts
$rossrv show AddTwoInts
$roscd rospy_tutorials
$ls
$cd srv
$ls
$more AddTwoInts.srv

creating a server and client py files in python :
$rosrun ros__tutorials add_server.py
$osrun ros__tutorials add_client.py 5 8

To make python file executable while running in the Linux bash terminal:
Prepend #! /usr/bin/python with your script.
Run the following command in your terminal to make the script executable: chmod +x SCRIPTNAME.py
Now, ​simply type ./SCRIPTNAME.py to run the executable script.

Motion in ROS(building robot cleaning application):
$catkin_create_pkg turtlesim_cleaner geometry_msgs rospy
$catkin_make

$rosrun turtlesim turtlesim_node
$rostopic list
$rostopic info /turtle1/cmd_vel
```
