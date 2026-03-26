# Setup (ROS 2)

Minimal required setup for this package.

## 1) Prerequisites

- Install ROS 2 for your Ubuntu version.
- Source ROS 2:

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
```

## 2) Required Dependencies

```bash
sudo apt update
sudo apt install -y \
	ros-$ROS_DISTRO-nmea-msgs \
	ros-$ROS_DISTRO-gps-msgs \
	libboost-all-dev \
	libpcap-dev \
	libgeographic-dev
```

If `libgeographic-dev` is not available (for example on Ubuntu 24.04), install:

```bash
sudo apt install -y libgeographiclib-dev
```

## 3) Build

From your ROS 2 workspace root (the folder containing `src`):

```bash
colcon build --packages-up-to septentrio_gnss_driver
source install/setup.bash
```

## 4) Run

Use the simple configuration file:

```bash
ros2 launch septentrio_gnss_driver rover.launch.py file_name:=simple_config.yaml
```

## Important Notes

- Before launch, adjust `config/rover.yaml` (or `config/gnss.yaml` / `config/ins.yaml`) for device connection and publish options.
- Unless `configure_rx` is set to `false`, the driver overwrites receiver settings from the YAML file.
- For serial devices, ensure your user is in the `dialout` group.
- `use_ros_axis_orientation: true` converts Septentrio NED orientation to ROS ENU orientation.
- If using multiple RTK/VSM ports with `keep_open: false`, increase SIGTERM timeout in launch files to allow clean port shutdown.
- Firmware baseline used for development/testing: GNSS >= 4.10.0, INS >= 1.3.2.
