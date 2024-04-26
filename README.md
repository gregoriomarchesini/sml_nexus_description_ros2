[![DHSG](https://img.shields.io/badge/KTH-DHSG-green)](https://www.example.com/dhsg) [![ROS2 Humble Version](https://img.shields.io/badge/ROS2-Humble-orange)](https://www.example.com/ros2)
# SML Nexus robot description
This is a ROS2 package for simulating the Nexus robots from the [Smart Mobiility Lab](https://www.kth.se/is/dcs/research/control-of-transport/smart-mobility-lab/smart-mobility-lab-1.441539) at KTH. The Nexus platform are holonomic robot receiving command in velocity under the topic `cmd_vel`.

This is a ROS2 package compatible with the `humble` distro. No other distro was tested for this package
For a comprehensive list of ROS2 tutorials please visit the ROS2 [main page](https://docs.ros.org/en/humble/index.html)


## Installation


In order to run the package make sure that `gazebo` and `rviz2` are correctly installed. We recommend using Gazebo Classic 11. In addition you will have to download the gazebo plug in required to run the nexus platforms.

```
git clone https://github.com/gregoriomarchesini/sml_nexus_plugin_ros2
```
Run rosdep before downloading the package

## Contacts 
Gregorio Marchesini  [gremar@kth.se](mailto:gremar@kth.se)
