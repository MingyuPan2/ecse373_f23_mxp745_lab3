# Lab 3: ROS Bags and Playback

## Overview

This lab will interpret the “ROS bagged” data from the architectural survey of the fifth floor of the Glennan building using the URDF description from the previous laboratory.

### Lab 3 Link

Lab 3 Link: [Laboratory #3_cert-1.pdf](https://canvas.case.edu/courses/38747/assignments/509272)

## Modelling Processes

### robot_state_publisher Update

Since the ROS bag from the survey includes transformation messages, robot_state_publisher does not need to be used, and the launch file was updated to include a new argument, use_robot_state_publisher, that has a default true value.

### Load the Previous Robot Model

#### -Create new .urdf and .xacro file-

In the new .xacro file, a base_link was defined to which everything must attach to. An IMU was added and connected to base_link. Follow the code on Page 2 of Lab 3 to define the appropriate .xacro file in the new package.

## Create Appropriate Launch File

In the package directory, create a folder named `launch`, then create a `display.launch` file in the `launch` folder. The .xacro file WILL NOT be used by default. The joint_gui WILL be used by default. Some code in <?ignore ... ?> were included for reference. Follow the code below to include everything in the file:

	<launch>
		<?ignore
		<arg name = "lab2robot" default = "urdf_from_xacro.urdf" />
		<arg name = "lab2robotfile" default = "$(find navvis_description)/urdf/$(arg urdf_from_xacro)" />
		<param name = "robot_description" textfile = "$(arg lab2robotfile)" />
		<node pkg = "robot_state_publisher" type = "robot_state_publisher" name = "robot_state_publisher" />
		<node pkg = "rviz" type = "rviz" name = "rviz" args = "-d $(find navvis_description)/config/config.rviz" required = "true" />
		?>

		<arg name="use_xacro" default="false" />
		<arg name="joint_gui" default="true" />
		<arg name="rviz_config_file" default="$(find navvis_description)/config/config.rviz" />
		<arg if="$(arg use_xacro)" name="filename" default="lab2robot.xacro" />
		<arg unless="$(arg use_xacro)" name="filename" default="urdf_from_xacro.urdf" />
		<arg name="file" default="$(find navvis_description)/urdf/$(arg filename)" />	
		<param if="$(arg use_xacro)" name="robot_description" command="$(find xacro)/xacro $(arg file)" /> 
		<param unless="$(arg use_xacro)" name="robot_description" textfile="$(arg file)" />
		<node pkg="robot_state_publisher" type="robot_state_publisher" name="robot_state_publisher" /> 
		<node pkg="rviz" type="rviz" name="rviz" args="-d $(arg rviz_config_file)" required = "true" />
		<node pkg ="joint_state_publisher_gui" type="joint_state_publisher_gui" name="joint_state_publisher_gui" if="$(arg joint_gui)" />
		<node pkg ="joint_state_publisher" type="joint_state_publisher" name="joint_state_publisher" unless="$(arg joint_gui)" />
	</launch>

## Using ROS Bag Files

Download the bag files to ubuntu. To play the bags, use the following command:

	rosbag play /home/mingyupan/Download/glennan_5_basic_short.bag /tf_trajectory:=/tf

## View Robot in RVIZ with Bag Files

FIrst, remember to update the .urdf file from the .xacro file using:

	rosrun xacro xacro lab2robot.xacro > whatever.urdf

To view the robot, first:

	cd catkin_ws
	source devel/setup.bash
	roscore &
	
To view robot WITH the joint_gui:
	
	roslaunch navvis_description display.launch &
	
To view robot WITHOUT the joint_gui:
	
	roslaunch navvis_description display.launch joint_gui:=false &

To USE .xacro file, add:

	roslaunch ... use_xacro:=true ... &
