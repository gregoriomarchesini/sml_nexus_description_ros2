[![DHSG](https://img.shields.io/badge/KTH-DHSG-green)](https://www.example.com/dhsg) [![ROS2 Humble Version](https://img.shields.io/badge/ROS2-Humble-orange)](https://www.example.com/ros2)






# Nexus Robot description Gazebo :robot:

This package contains the nexus robot description as an `.urdf` file for simulating the nexus robots in Gazebo Classic 11. The package was tested for the ROS2 Humble version. 

## The nexus platform
Nexus is an holonomic robot platform that is commonly applied for robotics research. You can find many different nexus models [here](https://www.nexusrobot.com/product/product-category/robot-kits). Every robot has different dynamics and applications. At KTH, the [mecanum wheel model](https://www.nexusrobot.com/product/4wd-mecanum-wheel-mobile-arduino-robotics-car-10011.html) is the one we commonly use



![Alt text](resource/nexus.png?raw=true "Title")

## What is this package for ?
This package contains some basic `launch.py` file to run the nexus robot in Gazebo 11. To run the robots it is required to have installed the main gazebo plugin for simulating the nexus. You can download the plugin here.

```
cd myworkspace/src
git clone https://github.com/gregoriomarchesini/sml_nexus_plugin_ros2.git
```


## Launching the model in gazebo
To spawn the nexus robot in gazebo all you have to do it is to make sure that this package and the `sml_nexus_plugin_ros2` are included in your work space. After you have built your workspace you can run the following command to spawn a single nexus in Gazebo (remember to source the overlay first with the command `source install/setup.sh`)


```
ros2 launch sml_nexus_description  sml_nexus_gazebo.launch.py 
```

Once spawned you should see one nexus spawned in the center the gazebo simulator scene.

## Understanding the process 
### Topics and nodes
You can check the topics the simulator is publishing to by running 
```
ros2 topic list 

/joint_states
/mocap_simulator/link_states_mocap
/mocap_simulator/model_states_mocap
/nexus/cmd_vel
/nexus/front_sensor
/nexus/left_sensor
/nexus/odom
/nexus/rear_sensor
/nexus/right_sensor
/parameter_events
/performance_metrics
/robot_description
/rosout
/tf
/tf_static
```
And you can check the nodes running 

```
ros2 node list 
/gazebo
/mocap_simulator/gazebo_ros_state
/nexus/gazebo_ros_front_sensor
/nexus/gazebo_ros_left_sensor
/nexus/gazebo_ros_rear_sensor
/nexus/gazebo_ros_right_sensor
/nexus/nexus
/robot_state_publisher

```

As you can see the nexus these a common name space `nexus` for all the nodes/topics related to the robot. The `mocap_simulator` is another ros node that can be used to get state information about the entities in the simulation. Although, it is recommended to use the `tf` topic to get the state of the robot if needed from another node. You can check the 

```
ros2 topic echo \tf

transforms:
- header:
    stamp:
      sec: 784
      nanosec: 901000000
    frame_id: world
  child_frame_id: nexus
  transform:
    translation:
      x: 0.012536082029753726
      y: -0.0030224165277399316
      z: -5.221080234381337e-06
    rotation:
      x: -1.2695464419606042e-05
      y: -4.065814654930434e-06
      z: 0.00010434655154104989
      w: 0.9999999944670457
---
```
### Controlling the robot 
To control the robot you can just run 

```
ros2 topic pub /nexus/cmd_vel geometry_msgs/msg/Twist '{linear: {x: 2, y: 1.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.0}}' -r 20
```

This should give it a good start to play with the nexus. You shouuld now explore how to create a controller to make your robot do some "smarter" manoeuvering rather than publishing the commands from the command 

### Changing the initial position of my robot 
In the launch file `sml_nexus.gazebo.launch.py` you can change the line 

```python
args =  ['-topic', '/robot_description',  # do not change this 
             '-entity', 'nexus',          # this is the name that gazebo will reserve for your model (this is also the name that you will see in the \mocap_simulator topic)
             '-z', '2.0', # change z coordinate
             '-x','3.0',  # change x coordinate
             '-y','1.0']  # change y coordinate

spawn_entity = Node(
        package='gazebo_ros',
        executable='spawn_entity.py',
        arguments=args,  # Separate arguments as individual strings
        output='screen',
        condition=IfCondition(LaunchConfiguration('verbose'))
    )
```





### How to change the namespace of the robot ?

In the launch file `sml_nexus.gazebo.launch.py` you can change the line 

```python
# Use xacro to process the file
xacro_file = os.path.join(get_package_share_directory(pkg_name), robot_xacro_subpath)
robot_description_config = xacro.process_file(xacro_file,mappings={"namespace":"nexus"}) # put here a cool name!
robot_desc = robot_description_config.toxml()  

```
### How can I go multi-agent?
To have multiple nexus spawned in the same simulation you need to check the launch filles in the `sml_tutorial_ros2` package [here](https://github.com/gregoriomarchesini/sml_nexus_tutorials_ros2.git). You can clone the package simply in your `src` folder

```
git clone https://github.com/gregoriomarchesini/sml_nexus_tutorials_ros2.git
```

More information is given there


## Requirements
The package was tested for ROS2 Humble. An installation of Gazebo versions 9-11 is required to launch the simulator. Gazebo 11 is the currently tested version.


## Installation 

If you haven't done so, create a ROS2 workspace 

```
mkdir -p nexus_ws/src
cd nexus_ws/src
```
from the `src` folder install the following repos

```
git clone https://github.com/gregoriomarchesini/sml_nexus_plugin_ros2.git
git clone https://github.com/gregoriomarchesini/sml_nexus_description_ros2.git
git clone https://github.com/gregoriomarchesini/sml_nexus_tutorials_ros2.git
```

Then build the workspace 

```
cd ..
colcon build --symlink-install
```
You are now ready to run your nexus. Remember to source the workspace overlay 

```
source install/setup.sh
```
and then 
```
ros2 launch sml_nexus_tutorials_ros2 sml_nexus_mas_gazebo.launch.py
```

You will see three robots 


Once you spawned your robot you can publish to the `cmd_vel` topic for each agent.

```
ros2 topic pub agent1/cmd_vel geometry_msgs/msg/Twist '{linear: {x: 0.2, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.2}}' -r 20
```


Happy coding! :smile:


## License

This package is distributed under the [Apache License 2.0](LICENSE).

## Authors

Gregorio Marchesini [gremar@kth.se](mailto:gremar@kth.se).
