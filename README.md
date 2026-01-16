# UR ros2 starter pack

## Requirements

- A linux host computer (Mac might work but it hasn't been tested)
- Docker
- Git
- VScode or Cursor

## Documentation

https://docs.universal-robots.com/Universal_Robots_ROS2_Documentation/doc/ur_robot_driver/ur_robot_driver/doc/index.html

## Preparing the host

### Allow X11 forwarding

```bash
echo 'xhost +local:docker' >> ~/.bashrc
source ~/.bashrc
```

### Configure git (if not configured already)

Create a ssh key if necessary, and set it up.

```bash
git config --global user.email "your-email@example.com"
git config --global user.name "Your Name"
```

Clone this repository

### Setup the network

In a nutshell this involves turning all network interfaces off, setting up an ethernet connection with Static IP, then turning the network interfaces back up again. Follow instructions here:

https://docs.universal-robots.com/Universal_Robots_ROS2_Documentation/doc/ur_client_library/doc/setup/network_setup.html

### Start the dev container

- Open this repository with vscode or cursor.
- When prompted, start the dev container.
- Start a terminal

## Extract calibration Information

Run:

```bash
ros2 launch ur_calibration calibration_correction.launch.py \
  robot_ip:=192.168.56.101 \
  target_filename:="${PWD}/my_robot_calibration.yaml"
```

# Running the robot

## Simulation mode with mock hardware

Start the robot driver

```bash
ros2 launch ur_robot_driver ur_control.launch.py \
  ur_type:=ur12e \
  use_mock_hardware:=true \
  headless_mode:=true \
  robot_ip:=192.168.56.101 \
  kinematics_params_file:="${PWD}/my_robot_calibration.yaml"
```

## Control with moveit and rviz

```bash
ros2 launch ur_moveit_config ur_moveit.launch.py \
  ur_type:=ur12e \
  launch_rviz:=true
```

## Running with real hardware

```bash
ros2 launch ur_robot_driver ur_control.launch.py \
  ur_type:=ur12e \
  use_mock_hardware:=false \
  headless_mode:=true \
  robot_ip:=192.168.56.101 \
  kinematics_params_file:="${PWD}/my_robot_calibration.yaml"
```

