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

## View Robot with 3 Configurations

Three are three configurations:

### Configuration 1: Original (no map loaded)

To view the original robot with no maps loaded, use the following command:

	roslaunch rosbags_playback lab3.launch &
	
When loaded, RVIZ will display the original robot model, with three lasers.

### Configuration 2: Target Frame-Base (map loaded)

To load the map, use the following:

	rosrun map_server map_server `rospack find maps_glennan`/maps/glennan5_map.yaml
	rostopic echo -n 1 /map

To load the bag file, use the following:

	rosbag play --clock /home/mingyupan/Downloads/gnennan-5-<basic/full>.bag /tf_trajectory:=/tf
	
To view the robot in RVIZ, use the following:

	roslaunch rosbags_playback lab3config1.launch &
	
When loaded, RVIZ will display the robot model with the map and laser scanned surrounding with decay time of 5. The view frame was set to "base" so the camera focuses on the robot constantly. 

#### Sim-time

Remember to "Reset" if necessary. 

If use_sim_time is TRUE (default), click "reset" will replay the bag file and allow the robot to begin from the very start.

If use_sim_time is FALSE, click "reset" will NOT replay the bag file and the robot will be in the position specified by the bag file at the time of reset.

### Configuration 3: Target Frame-Map (map loaded)

To load the map, use the following:

	rosrun map_server map_server `rospack find maps_glennan`/maps/glennan5_map.yaml
	rostopic echo -n 1 /map
	
To load the bag file, use the following:

	rosbag play --clock /home/mingyupan/Downloads/gnennan-5-<basic/full>.bag /tf_trajectory:=/tf
	
To view the robot in RVIZ, use the following:

	roslaunch rosbags_playback lab3config2.launch &
	
When loaded, RVIZ will display the robot model with the map and laser scanned surrounding with decay time of 5. The view frame was set to "map". 

#### Sim-time

Remember to "Reset" if necessary. 

### Other Modifications to Configurations Above

To Not use robot_state_publisher, add the following:

	roslaunch ... use_robot_state_publisher:=false ... &
	
To Not use the lab3robot.xacro file, add the following:

	roslaunch ... use_lab3robot_xacro:=false ... &

To NOT use the external clock (sim_time), add the following:

	roslaunch ... use_sim_time:=false ... &
