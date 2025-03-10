---
layout: default
title: Running the Software
parent: SMB Core Software
nav_order: 3
---

# How to Run SMB Software?
{:.no_toc}
This documentation explains the basic steps on how to run SMB software. 

Please [create an issue](https://github.com/ETHZ-RobotX/SuperMegaBot/issues/new) for any missing library, package, driver, error or any kind of unclear instruction.
{: .smb-mention }

* Table of contents
{:toc}

## Remark
{:.no_toc}

This document does not explain the full capability of the robot. It gives basic information related to the core software and how to run it. For more information please check the packages. 

In this document there are two terminal types:
1. Terminal on your PC: A terminal on your PC (called _host PC_). 
2. Terminal of the SMB: A terminal on the SMB, established via a SSH connection.

If you're planning to use the software on the robot, please be sure that you followed the [How to drive the SMB documentation](../robot-operation/HowToDriveTheSMB.md).
{: .smb-info }

To connect SMB please refer to the [HowToConnectSMB Document](../robot-operation/HowToConnectToSMB.md).
{: .smb-info }

## Use of Basic Functionality

You can run the software either in simulation or on the real robot. In both cases, you can control the robot manually (using the Logitch Joypad) or operate it in autonomous mode.

If you want to switch between modes make sure that you killed the previous mode.
{: .smb-info }

In both modes all sensor values, odometry and robot state can be read and visualized. In autonomous mode, you can give a goal position for the robot to navigate autonomously via the Rviz interface. 

### Simulation Mode
The simulation runs on the host PC (your computer). To run the simulation you do not need a connection to SMB.

```bash
# In the host pc
roslaunch smb_gazebo sim.launch launch_gazebo_gui:=true
```

### Autonomous Navigation in Simulation 

More information about the path planner can be found in the [path planner repository](https://github.com/VIS4ROB-lab/smb_path_planner).

Since we have successfully set up the SMB software, one can launch the simulation and get the first experience with the OMPL path planner.
Make sure that the `smb_navigation` is built. If not, run:
```bash
# In the host pc
catkin build smb_path_planner
```
then, launch the simulation environment:
```bash
# In the host pc
roslaunch smb_gazebo sim.launch launch_gazebo_gui:=true
```
Subsequently, in the second terminal window launch the OMPL path planner:
```bash
# In the host pc
roslaunch smb_navigation navigate2d_ompl.launch sim:=true global_frame:=tracking_camera_odom
```
In the RVIZ you should observe a grey-scaled map with SMB in the middle. Now, select `2d Navigation Goal` from the top toolbar and set the goal for the planner in the feasible region within the map.

To be able to use the advanced features of the path planner, refer to the [SMB path planner Wiki](https://github.com/VIS4ROB-lab/smb_path_planner/wiki).

### Running the software on the robot
#### Manual mode
If you want to switch between modes make sure that you killed the previous mode. 
{: .smb-info }

```bash
# In the terminal of SSH
roslaunch smb smb.launch

# If you see the message "First IMU Received", 
# everything started without any problem
```

You can test the sensor by reading the published topics.

To visualize the sensor reading via Rviz
```bash
# In the terminal of host PC
roslaunch smb_opc opc.launch

# You should see the robot model in Rviz
```

You might need to restart the base if the robot does not respond to the teleop control
{: .smb-info }

In order to control the SMB with the joystick, you should keep presing the L1 button while driving and use the left stick to control the robot.


#### Autonomous Mode
If you want to switch between modes make sure that you killed the other process.
{: .smb-info }

```bash
# In the terminal of SSH
roslaunch smb smb.launch

# If you see the message "First IMU Received", 
# everything started without any problem
```

To start the autonomous navigation please run the following terminal command in an other terminal window.
```bash
# In the terminal of SSH
roslaunch smb_navigation navigate2d_ompl.launch 

# If you see the message "odom received", 
# everything started without any problem
```

To visualize the sensor reading via Rviz
```bash
# In the terminal of host PC
roslaunch smb_opc opc.launch

# You should see the robot model in Rviz
```

After that you can use the Rviz 2D Navigation Goal tool 
