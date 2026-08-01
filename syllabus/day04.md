---
notion-url: https://www.notion.so/3aec4db880d9812e98e0c7abfc7b50d8
title: day04
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/day04-3aec4db880d9812e98e0c7abfc7b50d8
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Day 04: ROS2 Installation, DDS & Workspace Setup

### 📌 TL;DR

---

### 📖 The Core Topics

- **Why ROS2 over ROS1? (The Clean Break)**

	- **No Master Node**: ROS1 used `roscore` (single point of failure). ROS2 uses DDS (distributed auto-discovery).

	- **Real-time execution**: Built-in support for low-latency PREEMPT_RT kernels.

	- **Quality of Service (QoS)**: Fine-tune transport parameters for lossy wireless networks.

	- **First-class Windows/OSX support**: Multi-platform target compatibility.

- **The DDS & RMW Middleware Layer**

	- **DDS (Data Distribution Service)**: Industrial standard for real-time, publish-subscribe networking.

	- **RMW (ROS Middleware)**: The abstraction layer that translates ROS2 client calls into vendor-specific DDS configurations.

	- **DDS Vendors**:

- **FastDDS** (eProsima): Default in Humble, fast and customisable.

- **CycloneDDS** (Eclipse): Common alternative, lightweight and robust.

- **Installing ROS2 Humble on Ubuntu 22.04**

	- Add the GPG key and security authorization repositories:

		
```bash
sudo apt update && sudo apt install curl gnupg2 lsb-release -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

	- Setup apt source list:

		
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu$(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

	- Install desktop packages (rviz, tools, demos):

		
```bash
sudo apt update
sudo apt install ros-humble-desktop -y
```

- **Sourcing Environments: Underlay vs. Overlay**

	- **Underlay**: System-wide ROS2 installation (located at `/opt/ros/humble`). Source using:

		
```bash
source /opt/ros/humble/setup.bash
```

	- **Overlay**: Your custom workspace where you write project code (typically `~/ros2_ws`). Source using:

		
```bash
source ~/ros2_ws/install/setup.bash
```

	- *Important Rule*: Always source the underlay *before* compiling your overlay.

- **Network Isolation: Domain ID**

	- Multiple people on the same Wi-Fi running ROS2 will interfere with each other because DDS automatically connects to matching topics.

	- Prevent this by setting a unique domain ID for your robot:

		
```bash
export ROS_DOMAIN_ID=42
```

		- **The Colcon Build Tool**

	- Build command: `colcon build`.

	- **Crucial flag**: `colcon build --symlink-install`.

- Why use it? Links your Python source files instead of copying them to the install directory. This means when you edit your Python scripts, you don’t need to rebuild workspace to apply the changes!

---

### 🛠️ Hands-on Lab

1. Install ROS2 Humble on your Ubuntu setup.

1. Initialize your workspace structure:

	
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build --symlink-install
```

1. Source the setup script in your `~/.bashrc` to make it automatic:

	
```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```

---

### 📝 Assignment

- **Task**: Verify your environment installation. Generate a troubleshooting log document `ros2_install_log.md` showing your output for:

	
```bash
printenv | grep ROS
```

	