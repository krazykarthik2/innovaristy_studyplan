---
notion-url: https://www.notion.so/3aec4db880d9816fbfd8c44abca5368d
title: week1_foundations
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/week1_foundations-3aec4db880d9816fbfd8c44abca5368d
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Week 1: Ubuntu & ROS2 Foundations (Days 1–5)

---

## Day 1: Ubuntu & Linux Fundamentals

### 📖 Detailed Structured Syllabus

1. **Ubuntu Installation & Setup**

	- Choosing the right version (LTS releases e.g., 22.04 LTS Jammy Jellyfish / 24.04 LTS Noble Numbat matching ROS2 distributions).

	- Creating a bootable USB drive using Rufus or Ventoy.

	- Bios/UEFI configuration (Secure Boot disabling, boot order, fast startup options).

1. **Dual Boot vs. Virtual Machine (VM)**

	- Partitioning schemes (swap, root `/`, home `/home`, EFI system partition).

	- Virtualization choices: Oracle VirtualBox vs. VMware Workstation Player.

	- Allocating system resources (RAM, CPU cores, 3D Acceleration, virtual hard drive size).

1. **The Linux File System Hierarchy**

	- Root directory structure: `/bin`, `/etc` (configurations), `/home` (user space), `/var` (logs), `/opt` (third-party software like ROS2), `/dev` (device files).

	- Absolute vs. relative paths.

1. **Terminal Commands (CLI Essentials)**

	- Navigation: `pwd`, `ls` (flags: `l`, `a`, `h`), `cd`.

	- File operations: `mkdir`, `touch`, `cp`, `mv`, `rm` (flags: `r`, `f`).

	- Searching and viewing: `cat`, `less`, `head`, `tail`, `grep`, `find`.

	- System Monitoring: `top`, `htop`, `df`, `free`.

1. **File Permissions and Ownership**

	- Permissions model: Read (`r`), Write (`w`), Execute (`x`) for User (`u`), Group (`g`), Others (`o`).

	- Modifying permissions: `chmod` (symbolic vs. octal notation like `chmod +x` or `chmod 755`).

	- Ownership modification: `chown`, `chgrp`.

	- Administrative privileges: Using `sudo`, root user access, and editing `/etc/sudoers`.

1. **Secure Shell (SSH) Basics**

	- Installing SSH server: `sudo apt install openssh-server`.

	- Key generation (`ssh-keygen`) and copying (`ssh-copy-id`).

	- Remote login syntax: `ssh user@ip_address`.

1. **Terminal Text Editors**

	- **Nano**: Simple CLI editor, key shortcuts (`Ctrl+O` to write out, `Ctrl+X` to exit).

	- **Vim**: Modal editor, Insert/Command/Visual modes, basic search, and save (`:wq`, `:q!`).

1. **Package Manager (APT - Advanced Package Tool)**

	- Updating repositories: `sudo apt update`.

	- Upgrading software packages: `sudo apt upgrade`.

	- Installing/removing packages: `sudo apt install <pkg>`, `sudo apt purge <pkg>`.

	- PPA (Personal Package Archives) management.

### 🛠️ Hands-on Labs & Implementations

- **Lab 1.1: Install Ubuntu & Set Up Tools**

	- Perform a dual-boot setup or configure a VM with at least 4 CPU cores, 8GB RAM, and 50GB storage.

	- Install Visual Studio Code using the official `.deb` package or Apt:

		
```bash
sudo apt update
sudo apt install code
```

	- Install Git:

		
```bash
sudo apt install git-all
```

- **Lab 1.2: Terminal & Environment Customization**

	- Open `~/.bashrc` and add aliases to streamline terminal usage (e.g., `alias gs="git status"`).

	- Configure git global settings:

		
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 📝 Assignment

- **Task**: Create a comprehensive terminal commands cheat sheet containing at least 30 essential Linux command examples with brief explanations. Organize it under headers: Navigation, Management, Network, Permissions, and Shortcuts. Save it as `linux_cheat_sheet.md` in your workspace.

---

## Day 2: Linux for Robotics

### 📖 Detailed Structured Syllabus

1. **Process Management in Ubuntu**

	- Understanding process IDs (PID), parent/child processes.

	- Listing processes: `ps aux`, `pstree`, `pgrep`.

	- Monitoring resource usage: `top`, `htop`.

	- Process control signals: `kill`, `killall`, `pkill` (signals: `SIGTERM (15)`, `SIGKILL (9)`, `SIGHUP (1)`).

	- Background and foreground tasks: `&`, `bg`, `fg`, `jobs`.

1. **System Services (systemd & systemctl)**

	- What is a system service daemon?

	- Managing services: `sudo systemctl start|stop|restart|status <service>`.

	- Enabling/disabling services on boot: `sudo systemctl enable|disable <service>`.

	- Checking system logs: `journalctl` (filtering by service: `journalctl -u <service>`).

1. **Environment Variables**

	- Local vs. Global variables.

	- Exporting variables: `export MY_VAR="value"`.

	- System-wide configuration using `/etc/environment` or shell configuration files (`~/.bashrc`, `~/.profile`).

	- The `PATH` variable: How the OS finds binaries.

1. **Bash Scripting Foundations**

	- Script structure: Shebang (`#!/bin/bash`).

	- Declaring variables and utilizing control flow: `if/else`, `for` loops, `while` loops.

	- Reading command-line arguments (`$1`, `$2`, `$@`).

	- Functions, exit codes (`exit 0`, `exit 1`), and error handling (`set -e`).

1. **Automation and Task Scheduling**

	- Automating routine system checks and updates.

	- Introduction to `cron` jobs: Crontab syntax (`* * * * command`), scheduling periodic scripts.

1. **Linux Networking for Multi-Agent Robotics**

	- Checking network configurations: `ip a`, `ifconfig`, `netstat`, `ss`.

	- Testing network reachability: `ping`, `traceroute`.

	- Port scanning and firewall management: `nmap`, `ufw` (Uncomplicated Firewall).

1. **Static IP Configuration**

	- Understanding DHCP vs. Static IP.

	- Setting up static IPs in Ubuntu using Netplan configuration YAMLs (`/etc/netplan/*.yaml`).

1. **SSH & Remote Device Interfacing**

	- Connecting to remote microcontrollers/SBCs (Single Board Computers like Raspberry Pi/Jetson Nano).

	- Transferring files securely using `scp` and `rsync`.

	- Port forwarding and graphical interface forwarding (`ssh -X`).

### 🛠️ Hands-on Labs & Implementations

- **Lab 2.1: Write Bash Automation Scripts**

	- Develop a shell script that installs basic development tools (`build-essential`, `curl`, `git`, `htop`, `tmux`) in one execution.

- **Lab 2.2: Setup Static IP & SSH Control**

	- Configure a static IP address on your system.

	- Connect to another PC or VM on your local network using SSH and run a diagnostic check (e.g., memory and disk space utilization) remotely.

### 📝 Assignment

- **Task**: Create an Ubuntu Setup Automation Script (`ubuntu_setup.sh`). It must automatically update package indices, install a defined list of essential development libraries, set up custom shell configurations, verify system dependencies, and output a detailed status report. Ensure it includes comments explaining each section and error checking (`set -e`).

---

## Day 3: Python for Robotics

### 📖 Detailed Structured Syllabus

1. **Python Language Review**

	- Variable types, basic operators, and data structures (Lists, Tuples, Dictionaries, Sets).

	- Functions: definitions, default parameters, arguments (`args`, `*kwargs`), and docstrings.

	- List comprehensions and lambda functions.

1. **Virtual Environments**

	- Why isolate dependencies? System Python vs. project-specific runtimes.

	- Using Python `venv`: `python3 -m venv my_env`.

	- Activating/deactivating environments: `source my_env/bin/activate`.

	- Package management: `pip install`, `pip freeze > requirements.txt`, `pip install -r requirements.txt`.

1. **Object-Oriented Programming (OOP) in Robotics**

	- Classes and Objects.

	- Initializer/Constructor (`__init__`) and Instance variables/methods.

	- Inheritance: extending classes (critical for writing ROS2 nodes).

	- Encapsulation, static methods (`@staticmethod`), and property decorators.

1. **File Handling and Serialization**

	- Reading and writing raw text, CSV, and JSON configurations.

	- Utilizing standard libraries: `json`, `csv`, `os`, `sys`, `pathlib`.

1. **Multithreading, Multiprocessing, and Concurrency**

	- The Global Interpreter Lock (GIL) and its implications.

	- Implementing threads with the `threading` library (handling background I/O, listener loops).

	- Race conditions, locks (`threading.Lock()`), thread safety.

	- Asynchronous execution basics: `asyncio` framework.

1. **Exception and Error Handling**

	- Structured error handling: `try/except/finally` blocks.

	- Custom exception classes (e.g., `RobotSensorReadError`, `ActuatorFailureError`).

	- Logging warnings and errors: Python `logging` module.

### 🛠️ Hands-on Labs & Implementations

- **Lab 3.1: Python Sensor Data Simulator**

	- Create a Python application that runs a background thread generating simulated LiDAR/IMU data (ranges, orientation, angular velocity).

- **Lab 3.2: Python Motor Actuator Simulator**

	- Design a command-line interface running on a loop that receives target Cartesian coordinates, runs a simple simulation class to compute joint angles, and writes motor states to a mock telemetry file.

### 📝 Assignment

- **Task**: Create a Console-based Robot Simulator (`console_robot.py`). Define a `Robot` base class with parameters for position, orientation, and battery status. Implement inheritance to create a `SurgicalRobot` subclass adding specialized states (e.g., end-effector tool type, joint states). The simulator must run a loop accepting console inputs to move the robot, stream telemetry to a local file, handle out-of-bound errors using exceptions, and run a parallel thread updating the battery drain.

---

## Day 4: ROS2 Installation

### 📖 Detailed Structured Syllabus

1. **ROS2 Overview & Historical Context**

	- ROS1 vs. ROS2: Why the shift? (Real-time compatibility, DDS middleware, multi-robot systems, secure communication).

	- Architecture overview: Nodes, Executor, Middleware layer, OS abstraction.

1. **DDS (Data Distribution Service)**

	- What is DDS? Publish-Subscribe communication paradigm at the transport level.

	- Core features: Quality of Service (QoS), discovery mechanism, transport independence.

	- Major DDS vendors: eProsima FastDDS (default in Humble), CycloneDDS, RTI Connext.

1. **ROS2 Core Architecture Layers**

	- User Code Layer: C++ (`rclcpp`) and Python (`rclpy`) APIs.

	- ROS Client Library (RCL) interface written in C.

	- ROS Middleware (RMW) abstraction layer mapping RCL to specific DDS implementations.

1. **Supported ROS2 Distributions**

	- Humble Hawksbill (LTS, Ubuntu 22.04) — Recommended target.

	- Iron Irwini.

	- Jazzy Jalisco (LTS, Ubuntu 24.04).

1. **Environment Setup & Installation Mechanics**

	- Adding the ROS2 apt repository (GPG key verification, repository sources configuration).

	- Package selection: `ros-desktop` vs. `ros-base` (bare bones for microcontrollers/SBCs).

	- Sourcing environment setups: `/opt/ros/<distro>/setup.bash`.

1. **ROS2 Build Tool: Colcon**

	- Installing `colcon`: `sudo apt install python3-colcon-common-extensions`.

	- Directory structure for ROS2 development: `workspace_root/src/`.

	- Building workspace: `colcon build` (crucial flags: `-symlink-install` to avoid rebuilding on Python script changes).

### 🛠️ Hands-on Labs & Implementations

- **Lab 4.1: Source ROS2 Installation**

	- Install ROS2 Humble Hawksbill on your Ubuntu machine by following the Debian package instructions.

	- Add the sourcing script to your shell profile to automate environment setup:

		
```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

- **Lab 4.2: Create a Workspace and Build**

	- Create a clean ROS2 workspace:

		
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build
```

	- Source the overlay workspace setup:

		
```bash
source install/setup.bash
```

### 📝 Assignment

- **Task**: Document the entire ROS2 environment configuration in a file `ros2_install_log.md`. Include verification logs checking variable states:

	- `echo $ROS_DISTRO`

	- `echo $ROS_VERSION`

	- `printenv | grep ROS`
Document any compilation/link issues you ran into with Colcon and how you debugged/resolved them.

---

## Day 5: ROS2 Core Concepts

### 📖 Detailed Structured Syllabus

1. **ROS2 Nodes**

	- What is a node? Single-purpose executable (e.g., driver node, path planner node).

	- Creating a node in Python using `rclpy.node.Node`.

	- Execution lifecycle, initializing with `rclpy.init()` and spinning with `rclpy.spin()`.

1. **ROS2 Topics**

	- Unidirectional data stream (Publish/Subscribe).

	- Anonymity and loose coupling.

	- Topic namespacing.

1. **ROS2 Messages (msg)**

	- Strongly typed data structures.

	- Standard message types (`std_msgs`, `sensor_msgs`, `geometry_msgs`).

	- Custom message definition (.msg file layout, types, and generation).

1. **Publishers & Subscribers**

	- Creating a publisher: `create_publisher(msg_type, topic_name, qos_profile)`.

	- Creating a subscriber: `create_subscription(msg_type, topic_name, callback_function, qos_profile)`.

	- Understanding callbacks and executors.

1. **Command Line Interface (CLI) Tools**

	- Node inspection: `ros2 node list`, `ros2 node info <node_name>`.

	- Topic control and monitoring: `ros2 topic list`, `ros2 topic echo <topic>`, `ros2 topic info <topic>`, `ros2 topic hz <topic>`, `ros2 topic pub <topic> <type> <args>`.

	- Message structures: `ros2 interface show <msg_type>`.

### 🛠️ Hands-on Labs & Implementations

- **Lab 5.1: Write Python Talker and Listener Nodes**

	- Write a node `talker.py` that publishes string messages to `/chatter` at 10 Hz.

	- Write a node `listener.py` that subscribes to `/chatter` and logs the message payload.

- **Lab 5.2: Create a Custom Publisher Node**

	- Write a custom publisher that imports `geometry_msgs/msg/Pose` to publish coordinates simulating a surgical tool position.

### 📝 Assignment

- **Task**: Develop a Multi-Node Communication Project (`multi_node_chatter`). Write two custom nodes: a `TelemetrySource` node which reads simulated sensor values (IMU & Battery levels) and publishes them on separate topics (`/sensor/imu`, `/sensor/battery`), and a `TelemetryProcessor` node which subscribes to both topics, processes the input (filtering anomalies and tracking average states), and publishes a warning signal on `/alarms` if battery drops below 20%.

---

