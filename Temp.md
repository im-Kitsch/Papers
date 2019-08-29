# The pkg of HFL calibration
## Introduction

This is a ROS pkg written for testing parameters of HFL Lidar installation position. It receive the pointsclouds that is sent from HFL sensor, copy it, transform the pointcould in the new frame and republish it.

There are two version, offline calibration and online calibration. The difference is where the pointcloud come from -- rosbag or online data sent from sensor.

## About HFL                         
/ &nbsp; &nbsp; &nbsp;  --           &nbsp; &nbsp; &nbsp;     \ 
HFL1 HFL2 HFL3

You could see that there are three sensors with different positions and orientations. Then everyone has a field of view but they has overlapped part and this package is to align the overlapped part.

## Install the pkg
Please make sure that you have learned [the toturial for beginner level of ROS,](http://wiki.ros.org/ROS/Tutorials). 
1. Copy this folder to ROS_wrokspace/src.
2. Build it, (catkin_make or catkin build)

## How to run the pkg
### Offline: 
If you have recorded the sensor data in rosbag, just open `/.../hfl_calibri/launch/offline_calibri.launch` and `change your bagfile path in Line 4` . Then in terminal run: 
`$roslaunch hfl_calibri offline_calibration.launch`
### Online
If you are directly receiving the data from the vehicle, just open terminal and run:
`$roslaunch hfl_calibri online_calibration.launch`

After that you could type the position config of left and right HFL sensor in rqt. 
In linear, x y z is the offset of HFL sensor.
In angular, x y z is Yaw Pitch Roll.( To check)

Click the `□/debug/sub_tfx.....` to chang it to `☑/debug/sub_tfx..` to enable qrt sending message.

## More concrete description
Besides visualization of rviz, there are actually two nodes in this package, i.e. generator and talker.
Generator: subscribe the pointcloud msg -> copy it with new tf frame -> publish new pointcloud.
Talker: receive the info of position definition -> transform as tf info -> publish to tf_tree

#### Generator:
A template of running this node: 
    `rosrun hlf_calibri generator --frame_id  /debug/sub_tf1 --pcl_in /gfl110b2_01/points  --pcl_out /new_pcl1`

Meaning of keyword:
subscribe the pointcloud msg `from topic --pcl_in`
 -> 
copy it and change to new tf frame `from --frame_id`
->
 publish new pointcloud to `topic --pcl_out`
#### Talker 
A template of running this node:
     `rosrun hfl_calibri talker --frame_id_base /base_link --frame_id_new /debug/sub_tf1 --tf_topic /debug/sub_tf1/Twist'`

Meaning of keyword:
receive the info of position definition `from topic --tf_topic`
->
 transform as tf info 
->
 publish to tf_tree `with frame id --frame_id_new in reference to --frame_id_base`

#### rqt_gui
in rqt GUI you could define the position offset and publish it then the pointcloud are updated.



