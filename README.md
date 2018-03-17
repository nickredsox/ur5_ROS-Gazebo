#### 0. References
- [__`GitHub: utecrobotics/ur5`__](https://github.com/utecrobotics/ur5) testing ur5 motion 
- [__`Very useful ROS blog`__](http://www.guyuehome.com/column/ros-explore) ROS探索
- [__`ROS下如何使用moveit驱动UR5机械臂`__](http://blog.csdn.net/jayandchuxu/article/details/54693870)

#### 1. Universal Robot 5 Installation
[`Official installation tutorial`](http://wiki.ros.org/universal_robot)

The following command lines are for ur5 installation in ros-kinetic 
```angularjs
mkdir -p ur5_ws/src
cd ur5_ws/src
    
# retrieve the sources
git clone -b kinetic-devel https://github.com/ros-industrial/universal_robot.git
    
cd ~/ur5_ws
    
# checking dependencies 
rosdep install --from-paths src --ignore-src --rosdistro kinetic
   
# buildin, 
catkin_make

# if there is any error, try 
# pip uninstall em
# pip install empy 
   
# source this workspace (careful when also sourcing others)
cd ~/ur5_ws
source devel/setup.bash
```
To run UR5 in Gazebo and rviz (`source devel/setup.bash`)
```angularjs
roslaunch ur_gazebo ur5.launch limited:=true

roslaunch ur5_moveit_config ur5_moveit_planning_execution.launch sim:=true limited:=true

roslaunch ur5_moveit_config moveit_rviz.launch config:=true
```
To test UR5 motion, run [`testmotion.py`](src/testmotion.py)
Look into this link for [`straight line motion`](http://answers.gazebosim.org/question/15402/ur5-straight-line-motion/)

#### 2. Moveit
[`Official tutorial`](http://docs.ros.org/kinetic/api/moveit_tutorials/html/)
Install Moveit 
```
sudo apt-get install ros-kinetic-moveit
```
launch teh Moveit Setup Assistant
```
roslaunch moveit_setup_assistant setup_assistant.launch

1. Click on the "Create New MoveIt Configuration Package" button,click the "Browse" button, select the xacro file you created in the Previous Chapter, and click on the "Load Files" button.
PATH: /src/universal_robot/ur_description/urdf/ur5.urdf.xacro

2. Go to the "Self-Collisions" tab, and click on the "Regenerate Collision Matrix" button.

3. move to the "Virtual Joints" tab. Here, you will define a virtual joint for the base of the robot. Click the "Add Virtual Joint" button, and set the name of this joint to FixedBase, and the parent to world.

4. open the "Planning Groups" tab and click the "Add Group" button. Now, you will create a new group called manipulator, which uses the KDLKinematicsPlugin. 

3. Move Group Python InterFace Tutorial[`Official tutorial`](http://docs.ros.org/indigo/api/moveit_tutorials/html/doc/pr2_tutorials/planning/scripts/doc/move_group_python_interface_tutorial.html)


```

#### 3 USB Camera Installation in ROS
[`Reference link`](https://answers.ros.org/question/197651/how-to-install-a-driver-like-usb_cam/)

To list all video devices picked up by the kernel
```angularjs
ls -ltrh /dev/video*
```


```angularjs
cd ur5_ws/src

git clone https://github.com/bosch-ros-pkg/usb_cam.git

cd ..

catkin_make

source devel/setup.bash

roscd usb_cam

# run `roscore` in a new terminal
# Make sure a usb cam is connected
 
```

To connect external cam. 
Locate the usb_cam-test.launch file in folder 

`cd ~/ur5_ws/src/usb_cam/launch` 

Change 

`<param name="video_device" value="/dev/video0" />` 

to
 
`<param name="video_device" value="/dev/video1" />` 

From
 
`cd ~/catkin-ws/src/usb_cam/launch`
run
 
`roslaunch usb_cam-test.launch`

If this works, quit the test program, open rviz
```angularjs
rosrun rviz rviz
```
run the following command in ur5_ws folder (`source devel/setup.bash`)
```angularjs
rosrun usb_cam usb_cam_node
```

#### 4. Using Gazebo Camera Plugins

- [`Tutorial: Using a URDF in Gazebo`](http://gazebosim.org/tutorials/?tut=ros_urdf#Tutorial:UsingaURDFinGazebo)
prerequisite for Gazebo plugins
    
- [`Tutorial: Using Gazebo plugins with ROS`](http://gazebosim.org/tutorials?tut=ros_gzplugins)
learning to use Gazebo plugins


- [`Viewer for ROS image topics`](http://wiki.ros.org/image_view)


- [`Camera implementation with IRIS drone`](http://discuss.px4.io/t/how-to-add-a-ros-camera-to-iris-for-gazebo-simulation/5118)


- [`ur_description wiki`](http://wiki.ros.org/ur_description)
To view and manipulate the arm models in rviz, install the package from package management and launch the following:
```angularjs
roslaunch ur_description ur5_upload.launch
roslaunch ur_description test.launch
```
You probably need to install urdf_tutorial:
```angularjs
cd ~/catkin_ws/src/
git clone https://github.com/ros/urdf_tutorial
cd ..
catkin_make
source devel/setup.bash
```

In rviz, the first task	is to choose the frame of reference	for	the	visualization. In left panel, __`Global Options/Fixed Frame`__, 
choose a proper frame (e. g. __`world`__)

Next, we want to view the 3D model of the robot. To accomplish this, we will insert an instance of the `robot model` plugin
To add the robot model to the rviz scene, click the “Add” button and choose __`RobotModel`__

To test UR5 USB cam, run [`testvision.py`](src/testvision.py)

<p align="center">
<img src="https://github.com/lihuang3/ur5_notebook/blob/master/media/ezgif.com-video-to-gif.gif" width="600">
</p>
 	
#### 5. Revolute-Revolute Manipulator Robot
"[__`RRBot`__](https://github.com/ros-simulation/gazebo_ros_demos), or ''Revolute-Revolute Manipulator Robot'', 
is a simple 3-linkage, 2-joint arm that we will use to demonstrate various 
features of Gazebo and URDFs. 
It essentially a double inverted pendulum and demonstrates some fun 
control concepts within a simulator."

To get RRBot, clone the gazebo_ros_demos Github repo into the `/src` folder of your catkin workspace and rebuild your workspace:
```angularjs
cd ~/catkin_ws/src/
git clone https://github.com/ros-simulation/gazebo_ros_demos.git
cd ..
catkin_make
source devel/setup.bash
```

__Quick start__

Rviz:
```angularjs
roslaunch rrbot_description rrbot_rviz.launch
```

Gazebo:
```angularjs
roslaunch rrbot_gazebo rrbot_world.launch
```

[ROS Control](http://gazebosim.org/tutorials?tut=ros_control):
```angularjs
roslaunch rrbot_control rrbot_control.launch
```
Example of Moving Joints:
```angularjs
rostopic pub /rrbot/joint2_position_controller/command std_msgs/Float64 "data: -0.9"
```


