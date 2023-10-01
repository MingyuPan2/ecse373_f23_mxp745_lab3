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

## Using ROS Bag Files

Download the bag files to ubuntu. To play the bags, use the following command:

	rosbag play --clock /home/mingyupan/Download/glennan_5_<size>.bag /tf_trajectory:=/tf

This will start the bag playback.

## View the Map

### Download Map Server

Use the following command to first download the map_server package:

	sudo apt install ros-noetic-map-server

### Clone Glennan Map

The map of Glennan needs to be cloned to be viewed on the computer. Use the following command:

	git clone https://github.com/cwru-eecs-373/maps_glennan.git

Remember to catkin_make after download.

### map_server Package

Start the map_server node with the existing 2D map of the 5th floor of Glennan using the folowing command:

	rosrun map_server map_server `rospack find maps_glennan`/maps/glennan5_map.yaml

The message needs to be echoeed to /map to be viewed in RVIZ. Use the following command:

	rostopic echo -n 1 /map

### View Map in RVIZ

At the bottom left of the screen in RVIZ, press "Add" and choose "By topic" and select /map.

Now, the 2D map is loaded into  RVIZ. 

Under "Global Options", set the "Fised Frame" to map.

Under all three "LaserScan", set the "Decay Time" to 5-10. This will get laggy without a dedicated graphics card.


