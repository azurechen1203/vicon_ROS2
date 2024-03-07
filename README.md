# Vicon ROS2

**vicon_receiver** is a ROS 2 package, written in C++, that retrieves data from Vicon software and publishes it on ROS 2 topics. The code is partly derived from a mixture of [ROS2Vicon](https://github.com/aheuillet/Vicon-ROS2), [Vicon bridge](https://github.com/ethz-asl/vicon_bridge) and [ros2-vicon-receiver](https://github.com/OPT4SMART/ros2-vicon-receiver)

This is NOT an official ROS 2 package and is not supported. The package has been successfully tested with ROS 2 Humble on the operating systems Ubuntu 22.04.

## Requirements

### Installation of dependencies

Make sure you have ROS 2 installed (follow the instructions at the [ROS 2 website](https://index.ros.org/doc/ros2/Installation/)).

Then, install [Colcon](https://colcon.readthedocs.io/en/released/index.html) and [CMake](https://cmake.org/) :
```
$ sudo apt install -y python3-colcon-common-extensions cmake
```

### Installation of Datastream SDK and other libraries

The Datastream SDK libraries are required to be installed in the system. You can find them on [the official website](https://www.vicon.com/software/datastream-sdk/?section=downloads).

This package is shipped with Datastream SDK 1.12 (the latest version at the time of writing). If you are running Linux x64 and you want to install this version, simply `cd` into the main project folder and issue the command
```
$ ./install_libs.sh
```

## Quick start

### Clone the repo:
```
git clone git@github.com:azurechen1203/vicon_ROS2.git vicon_receiver
```

### Source the ROS2 workspace:
```
source /opt/ros/humble/setup.bash
```

### Building the package

Enter the project folder and build the executable
```
cd vicon_receiver
colcon build --symlink-install
source install/setup.bash
```

### Testing the package and interface are successfully created
```
ros2 pkg list | grep vicon
ros2 interface list | grep vicon
```

### Running the program

Open the launch file and edit the parameters. Running `colcon build` is not needed because of the `--symlink-install` option previously used.

Now you can the program with
```
ros2 launch vicon_receiver client.launch.py
```

Exit the program with CTRL+C.

### Information on ROS 2 topics and messages

The **vicon_receiver** package creates a topic for each segment in each subject with the pattern `namespace/subject_name/segment_name`. Information is published on the topics as soon as new data is available from the vicon client (typically at the vicon client frequency). The message type [Position](vicon_ROS2/msg/Position.msg) is used.

Example: suppose your namespace is the default `vicon` and you have two subjects (`subject_1` and `subject_2`) with two segments each (`segment_1` and `segment_2`). Then **vicon-ROS2** will publish messages on the following topics:
```
vicon/subject_1/segment_1
vicon/subject_1/segment_2
vicon/subject_2/segment_1
vicon/subject_2/segment_2
```



