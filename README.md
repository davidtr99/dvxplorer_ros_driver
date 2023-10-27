# DVXplorer ROS Driver
This package provides a ROS driver for the DVXplorer event camera. It is based on the driver provide inside of [rpg_dvs_ros](https://github.com/uzh-rpg/rpg_dvs_ros) package with some changes to make it compile as an isolated package.

The main dependencies of this package are:
- **dvs_msgs**: the required generic message package for event cameras. 
- **libcaer**: the library to communicate with the DVXplorer camera.
```
sudo apt install libcaer-dev
```

## Usage

The usage of this driver is as simple as connecting the camera via USB and launching:
```
rosrun dvxplorer_ros_driver dvxplorer_ros_driver_node   
```
You can also edit internal parameters through rqt_reconfigure:
```
rosrun rqt_reconfigure rqt_reconfigure 
```

