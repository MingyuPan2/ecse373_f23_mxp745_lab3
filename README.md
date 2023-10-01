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

## Using ROS Bag Files

Download the bag files to ubuntu. To play the bags, use the following command:

	rosbag play /home/mingyupan/Download/glennan_5_basic_short.bag /tf_trajectory:=/tf

## View Robot in RVIZ with Bag Files
