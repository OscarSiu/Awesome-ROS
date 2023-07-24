# ROS1 Tutorial

Initiated by Oscar Siu

**Last update on 24 AUG 2022**



**Prerequisite**
Ubuntu 20.04

## Installation
[Ubuntu install of ROS Noetic](http://wiki.ros.org/noetic/Installation/Ubuntu)
`rosversion -d`

search for usable package
`apt search ros-noetic` OR `sudo apt-get install ros-noetic-[tab]`

### Filesystem tools
`rospack`
`roscd`
`rosls`
`rosed`
`rosdep`

## Workspace Environment Setup
`roscore` OR skip to `roslaunch`

Edit .bashrc file
```
export EDITOR='nano -w'
export LIBGL_ALWAYS_INDIRECT=0
export LIBGL_ALWAYS_SOFTWARE=1

source /opt/ros/noetic/setup.bash
source ~/catkin_ws/devel/setup.bash
```
 `source ~/.bashrc`

Check workspace
`echo $ROS_PACKAGE_PATH`

Build package
```
source /opt/ros/noetic/setup.bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace

catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3
source devel/setup.bash
```

For every package, edit ==CMakelist.txt== and ==package.xml==
`catkin_create_pkg [package name] std_msgs rospy roscpp`

```
catkin build
catkin clean
catkin config
```

## Turtlebot Tutorial
Install tutorials package
`sudo apt-get install ros-noetic-ros-tutorials`

Run turtlesim
`roscore / rosmaster --core`
**`rosrun turtlesim turtlesim_node __name:=my_turtle`**


Tele-operation
`rosrun turtlesim turtle_teleop_key`

### ROS node 
`rosnode list`
`rosnode ping my_turtle`

###  ROS Topic	
`rosrun rqt_graph rqt_graph`

`rostopic echo /turtle1/cmd_vel`
`rostopic type /turtle1/cmd_vel`
`rosmsg show geometry_msgs/Twist`

Read laser scan data
`rostopic echo -n1 /scan`

**Publisher**
`rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'`

### Manipulation
`rosservice call`
`rosparam set`

Debug
`rosrun rqt_console rqt_console`
`rosrun rqt_logger_level rqt_logger_level`

Launch
`roslaunch beginner_tutorials turtlemimic.launch`

Record published files
`rosbag record -a`
`time rosbag play info [bag name]`

Troubleshoot
`roswtf`

## Turtlebot3
`sudo apt-get install ros-melodic-turtlebot3`

https://emanual.robotis.com/docs/en/platform/turtlebot3/slam_simulation/

## ROBOTIS tutorial
https://github.com/ROBOTIS-GIT/ros_tutorials
```
rosrun ros_tutorials_topic topic_publisher
rosrun ros_tutorials_topic topic_subscriber

rosrun ros_tutorials_service service_server
rosrun ros_tutorials_service service_client 2 5

rosservice call /ros_tutorial_srv 5 15

rosparam set /calculation_method 2
```

## Velodyne simulator

```
sudo apt-get install ros-noetic-velodyne-simulator
cd catkin_ws/src
git clone https://bitbucket.org/DataspeedInc/velodyne_simulator.git
catkin_make
```
`roslaunch velodyne_description example.launch`

`rosrun velodyne_laserscan velodyne_laserscan_node`

https://bitbucket.org/DataspeedInc/velodyne_simulator/src/master/
https://codediversion.wordpress.com/tag/lidar/
## Autonomous Obstacle Avoidance Robot
`roslaunch testbot_description testbot_gazebo.launch`

Tele-Op control
`roslaunch testbot_description teleop.launch`

Autonomous mode
`rosrun testbot_description sensor_data_listener.py`

`rostopic echo /cmd_vel`

https://www.youtube.com/watch?v=wcXuUG4BWAU
https://github.com/vibhuthasak/Obstacle_Avoidance_ROS

## USB Camera package
https://devblogs.microsoft.com/commandline/connecting-usb-devices-to-wsl/

```
sudo apt-get install ros-melodic-usb-cam
rosrun usb_cam usb_cam_node_device:=/dev/video?
rosrun image_view image_view image:=/camera/rgb/image_color
rqt_image_view image:=/image_raw

# export ROS_MASTER_URI = http://IP:11311
# export ROS_HOSTNAME = IP 
```

## AgileX Robotics Limo
Start the base node for limo
`roslaunch limo_bringup limo_start.launch`

Start the keyboard tele-op node
`roslaunch limo_bringup limo_teleop_keyboard.launch`

https://github.com/agilexrobotics/limo_ros

ydlidar package
https://github.com/YDLIDAR/ydlidar_ros


## ROS on Docker 
```
docker run -it --rm --name master ros:melodic
roscore

#Separate terminal
docker exec -it [master container ID] bash
source /opt/ros/melodic/setup.bash

#Publisher
rostopic pub /test std_msgs/String "data: '123'" -r1
#Subscriber
rostopic echo /test
```

`docker run --name  r1_noetic -e DISPLAY=host.docker.internal:0.0 -it osrf/ros:noetic-desktop`

## Useful links
[ROS documentation](https://wiki.ros.org/)

[ROS Cheat sheet](https://w3.cs.jmu.edu/spragunr/CS354_S19/handouts/ROSCheatsheet.pdf)

[Gazebo models](https://github.com/osrf/gazebo_models)
`git clone https://github.com/osrf/gazebo_models`

[Programming for Robotics - ROS](https://rsl.ethz.ch/education-students/lectures/ros.html)

[ROS1 waffle robot](https://ithelp.ithome.com.tw/articles/10251679)

[PoJen Lai](https://pojenlai.wordpress.com/)

[Read laserscan](https://www.theconstructsim.com/read-laserscan-data/?utm_source=youtube&utm_medium=ros_qa&utm_campaign=31)

URDF

https://wiki.ros.org/urdf/Tutorials
http://docs.ros.org/en/kinetic/api/moveit_tutorials/html/doc/getting_started/getting_started.html

[Navigation](http://wiki.ros.org/navigation/Tutorials)

[Racecar sim on docker](https://github.com/SoftArch/racecar-gazebo)

[Snap](http://wiki.ros.org/ROS/Tutorials/Packaging%20your%20ROS%20project%20as%20a%20snap)

## Successful built
https://github.com/richardw05/mybot_ws
https://github.com/gaunthan/Turtlebot2-On-Melodic


```
# create a new environmnet named ros  
conda create --name ros --channel conda-forge  
   ros-core \  
   ros-actionlib \  
   ros-dynamic-reconfigure  
   python=X.X  # select your desired Python version here (2.7, 3.6, 3.7)conda activate ros  
...  
now you can use any ros command from the core set, such as   
roscore  
roslaunch ...   
rostopic ...  
rosnode ...

```
------------------------

## Clearpath robotics
```
#Install
sudo apt-get update
sudo apt-get install ros-noetic-husky-desktop
sudo apt-get install ros-noetic-husky-simulator

roslaunch husky_gazebo husky_empty_world.launch
roslaunch husky_viz view_robot.launch

rostopic pub /husky_velocity_controller/cmd_vel geometry_msgs/Twist -r 1 '[0.0, 0.0, 0.0]' '[0.0, 0.0, 1.0]'

rosrun rqt_graph rqt_graph
rosrun tf view_frames
evince frames.pdf
```

Move Base Demo
```
export HUSKY_GAZEBO_DESCRIPTION=$(rospack find husky_gazebo)/urdf/description.gazebo.xacro

roslaunch husky_gazebo husky_playpen.launch
roslaunch husky_viz view_robot.launch
roslaunch husky_navigation move_base_mapless_demo.launch
```


