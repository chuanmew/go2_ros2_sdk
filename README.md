![Ros2 SDK](https://github.com/abizovnuralem/go2_ros2_sdk/assets/33475993/49edebbe-11b6-49c6-b82d-bc46257674bd)

# Welcome to the Unitree Go2 ROS2 SDK Project!

[![IsaacSim](https://img.shields.io/badge/IsaacSim-4.0-silver.svg)](https://docs.omniverse.nvidia.com/isaacsim/latest/overview.html)
[![Python](https://img.shields.io/badge/python-3.10-blue.svg)](https://docs.python.org/3/whatsnew/3.10.html)
[![Linux platform](https://img.shields.io/badge/platform-linux--64-orange.svg)](https://releases.ubuntu.com/22.04/)
[![Windows platform](https://img.shields.io/badge/platform-windows--64-orange.svg)](https://www.microsoft.com/en-us/)
![ROS2 Build](https://github.com/abizovnuralem/go2_ros2_sdk/actions/workflows/ros_build.yaml/badge.svg)
[![License](https://img.shields.io/badge/license-BSD--2-yellow.svg)](https://opensource.org/licenses/BSD-2-Clause)

We are happy to present you our integration of the Unitree Go2 with ROS2 over Wi-Fi, that was designed by the talented [@tfoldi](https://github.com/tfoldi). You can explore his groundbreaking work at [go2-webrtc](https://github.com/tfoldi/go2-webrtc).

This repo will empower your Unitree GO2 AIR/PRO/EDU robots with ROS2 capabilities, using both WebRTC (Wi-Fi) and CycloneDDS (Ethernet) protocols.

## Project RoadMap:

1. URDF :white_check_mark: 
2. Joint states sync in real time :white_check_mark: 
3. IMU sync in real time :white_check_mark: 
4. Joystick control in real time :white_check_mark: 
6. Go2 topics info in real time :white_check_mark: 
7. Foot force sensors info in real time :white_check_mark: 
8. Lidar stream (added pointCloud2) :white_check_mark: 
9. Camera stream :white_check_mark:
10. Foxglove bridge :white_check_mark:
11. Laser Scan :white_check_mark:
12. Multi robot support :white_check_mark:
13. WebRTC and CycloneDDS support :white_check_mark:
14. Creating a PointCloud map and store it :white_check_mark:
15. SLAM (slam_toolbox) (in the current version is not working, need to fix params)
16. Navigation (nav2) (in the current version is not working, need to fix params)
17. Object detection :white_check_mark:
18. AutoPilot

## Your feedback and support mean the world to us. 

If you're as enthusiastic about this project as we are, please consider giving it a :star: star!!!

Your encouragement fuels our passion and helps us develop our RoadMap further. We welcome any help or suggestions you can offer!

Together, let's push the boundaries of what's possible with the Unitree Go2 and ROS2!

## Exciting Features:

:sparkles: Full ROS2 SDK support for your Unitree GO2

:robot: Compatible with AIR, PRO, and EDU variants

:footprints: Access to foot force sensors feedback (available on some GO2 PRO models or EDU)


## Real time Go2 Air/PRO/EDU joints sync:

<p align="center">
<img width="1280" height="640" src="https://github.com/abizovnuralem/go2_ros2_sdk/assets/33475993/bf3f5a83-f02b-4c78-a7a1-b379ce057492" alt='Go2 joints sync'>
</p>

## Go2 Air/PRO/EDU lidar point cloud:

<p align="center">
<img width="1280" height="640" src="https://github.com/abizovnuralem/go2_ros2_sdk/assets/33475993/9c1c3826-f875-4da1-a650-747044e748e1" alt='Go2 point cloud'>
</p>


## System requirements
Tested systems and ROS2 distro
|systems|ROS2 distro|Build status
|--|--|--|
|Ubuntu 22.04|iron|![ROS2 CI](https://github.com/abizovnuralem/go2_ros2_sdk/actions/workflows/ros_build.yaml/badge.svg)
|Ubuntu 22.04|humble|![ROS2 CI](https://github.com/abizovnuralem/go2_ros2_sdk/actions/workflows/ros_build.yaml/badge.svg)
|Ubuntu 22.04|rolling|![ROS2 CI](https://github.com/abizovnuralem/go2_ros2_sdk/actions/workflows/ros_build.yaml/badge.svg)


## Installation

```shell
mkdir -p ros2_ws/src
cd ros2_ws/src
git clone --recurse-submodules https://github.com/abizovnuralem/go2_ros2_sdk.git
cp -a go2_ros2_sdk/. .
rm -r -f go2_ros2_sdk
sudo apt install ros-humble-image-tools
sudo apt install ros-humble-vision-msgs
sudo apt install python3-pip clang
pip install -r requirements.txt
cd ..
```

NOTE: check for any error messages, and do not disregard them. If `pip install` does not complete cleanly, various features will not work. For example, `open3d` does not yet support `python3.12` and therefore you will need to set up a 3.11 `venv` first etc.

Install `rust` language support in your system: [instructions](https://www.rust-lang.org/tools/install) 

`cargo` should work in terminal
```shell
cargo --version
```

Build `go2_ros_sdk`. You need to have `ros2` and `rosdep` installed. If you do not: [instructions](https://docs.ros.org/en/humble/Installation.html). Then:
```shell
source /opt/ros/$ROS_DISTRO/setup.bash
rosdep install --from-paths src --ignore-src -r -y
colcon build
```

## Usage
Don't forget to set up your Go2 robot in Wifi-mode and obtain the IP. You can use the mobile app to get it, go to Device -> Data -> Automatic Machine Inspection and look for STA Network: wlan0.

```shell
source install/setup.bash
export ROBOT_IP="robot_ip"
export CONN_TYPE="webrtc"
ros2 launch go2_robot_sdk robot.launch.py
```

## Real time image detection and tracking

<p align="center">
<img width="300" src="https://github.com/abizovnuralem/go2_ros2_sdk/doc_images/go2_air_giraffe.png" alt='Giraffe Detection and Tracking'>
</p>

This capability is directly based on Once you have launched the system, the color image data will be available at `go2_camera/color/image`. In another terminal enter:

```bash
source install/setup.bash
ros2 run coco_detector coco_detector_node
```

There will be a short delay the first time the node is run for PyTorch TorchVision to download the neural network. You should see a downloading progress bar. This network is then cached for subsequent runs.

On another terminal, to view the detection messages:
```shell
source install/setup.bash
ros2 topic echo /detected_objects
```

To view the image stream annotated with the labels and bounding boxes:
```shell
ros2 run image_tools showimage --ros-args -r /image:=/annotated_image
```

Example Use:

```shell
ros2 run coco_detector coco_detector_node --ros-args -p publish_annotated_image:=False -p device:=cuda -p detection_threshold:=0.7
```

This will run the coco detector without publishing the annotated image (it is True by default) using the default CUDA device (device=cpu by default). It sets the detection_threshold to 0.7 (it is 0.9 by default). The detection_threshold should be between 0.0 and 1.0; the higher this number the more detections will be rejected. If you have too many false detections try increasing this number. Thus only Detection2DArray messages are published on topic /detected_objects.

## 3D map generation
To save the map, you need to:

```shell
export MAP_SAVE=True
export MAP_NAME="3d_map"
```

Every 10 seconds, the map will be save to root folder of the repo.

## Multi robot support 
If you want to connect several robots for collaboration:

```
export ROBOT_IP="robot_ip_1, robot_ip_2, robot_ip_N"
```

## Switching between webrtc connection (Wi-Fi) to CycloneDDS (Ethernet)
```
export CONN_TYPE="webrtc"
```
or
```
export CONN_TYPE="cyclonedds"
```

## Foxglove

<p align="center">
<img width="1200" height="630" src="https://github.com/abizovnuralem/go2_ros2_sdk/assets/33475993/f0920d6c-5b7a-4718-b781-8cfa03a88095" alt='Foxglove bridge'>
</p>

To use Foxglove, you need to install Foxglove Studio:
```
sudo snap install foxglove-studio
```

1. Open Foxglove Studio and press "Open Connection".
2. In the "Open Connection" settings, choose "Foxglove WebSocket" and use the default configuration ws://localhost:8765, then press "Open".

## WSL 2

If you are running ROS2 under WSL2 - you may need to configure Joystick\Gamepad to navigate the robot.

1. Step 1 - share device with WSL2

    Follow steps here https://learn.microsoft.com/en-us/windows/wsl/connect-usb to share your console device with WSL2

2. Step 2 - Enable WSL2 joystick drivers

    WSL2 does not come by default with the modules for joysticks. Build WSL2 Kernel with the joystick drivers. Follow the instructions here: https://github.com/dorssel/usbipd-win/wiki/WSL-support#building-your-own-wsl-2-kernel-with-additional-drivers  If you're comfortable with WSl2, skip the export steps and start at `Install prerequisites.`

    Before buiding, edit `.config` file and update the CONFIG_ values listed in this GitHub issue: https://github.com/microsoft/WSL/issues/7747#issuecomment-1328217406

2. Step 3 - Give permissions to /dev/input devices

    Once you've finished the guides under Step 3 - you should be able to see your joystick device under /dev/input

    ```bash
    ls /dev/input
    by-id  by-path  event0  js0
    ```

    By default /dev/input/event* will only have root permissions, so joy node won't have access to the joystick

    Create a file `/etc/udev/rules.d/99-userdev-input.rules` with the following content:
    `KERNEL=="event*", SUBSYSTEM=="input", RUN+="/usr/bin/setfacl -m u:YOURUSERNAME:rw $env{DEVNAME}"`

    Run as root: `udevadm control --reload-rules && udevadm trigger`

    https://askubuntu.com/a/609678

3. Step 3 - verify that joy node is able to see the device properly. 

    Run `ros2 run joy joy_enumerate_devices`

    ```
    ID : GUID                             : GamePad : Mapped : Joystick Device Name
    -------------------------------------------------------------------------------
    0 : 030000005e040000120b000007050000 :    true :  false : Xbox Series X Controller
    ```

## Thanks

Special thanks to @tfoldi, @legion1581, @budavariam, @alex.lin and TheRoboVerse community!

## License

This project is licensed under the BSD 2-clause License - see the [LICENSE](https://github.com/abizovnuralem/go2_ros2_sdk/blob/master/LICENSE) file for details.
