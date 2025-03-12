# z1_sdk

Package for the Unitree Z1 SDK.

## Installation

**Note**: read all documentation listed at the bottom of this page before attempting to install and use this package.

### Non-ROS Setup (do not attempt if using ROS)

1. Install the [z1_controller](https://github.com/ori-drs/z1_controller) package (non-ROS setup).
1. Clone this repository:
	```bash
	git clone git@github.com:ori-drs/z1_sdk.git
	```
1. Build the executibles:
    ```bash
	cd z1_sdk/ && \
	rm -rf build && \
	mkdir build && \
	cd build && \
	cmake .. && make
	```

You should now be able to start the arm controller and command the real arm with an example program using the SDK by doing the following:
1. Navigate to the `z1_controller/build` folder you created and run `./z1_ctrl`.
1. Navigate to the `z1_sdk/build` folder you created and run `./lowcmd_development`.

### ROS Setup
1. Install the [z1_controller](https://github.com/ori-drs/z1_controller) package (ROS setup).
1. Clone this repository:
	```bash
	git clone git@github.com:ori-drs/z1_sdk.git
	```
2. Build the package:
    ```bash
	catkin build
	```

You should now be able to start the arm controller and command the arm with an example program using the SDK by doing the following:
1. - Real arm: `ROS_NAMESPACE=<your_arm_namespace> rosrun z1_controller z1_ctrl`
    - Gazebo arm: `ROS_NAMESPACE=<your_arm_namespace> rosrun z1_controller sim_ctrl`
1. `ROS_NAMESPACE=<your_arm_namespace> rosrun z1_sdk lowcmd_development`

## Documentation

- [Original](http://dev-z1.unitree.com) (official docs)
- [ORI](https://oxfordrobotics.atlassian.net/wiki/spaces/DRSC/pages/29400010/Unitree+Z1+Arm) (corrections, updates and changes -- read this after the original docs)