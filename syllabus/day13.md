---
notion-url: https://www.notion.so/3aec4db880d98145b126d406c7220631
title: day13
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/day13-3aec4db880d98145b126d406c7220631
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Day 13: Dobot Magician ROS2 Integration & Serial Driver

### 📌 TL;DR

---

### 📖 The Core Topics

- **Hardware Overview: Dobot Magician**

	- A 4-DoF desktop arm (Base rotate, Rear arm, Forearm, End-effector rotate).

	- Controlled via serial protocol over USB.

- **Serial Comm in Linux (**`**/dev/tty***`**)**

	- Connecting USB creates serial files: `/dev/ttyUSB0` or `/dev/ttyACM0`.

	- Fix permission issues by adding your user account to the `dialout` group (so you can read/write to the USB port):

		
```bash
sudo usermod -a -G dialout $USER
```

		- **Writing the Serial Driver Node in Python**

	- Uses the `pyserial` library to send raw byte arrays.

	- The node needs two main functions:

1. **Publisher Loop**: Periodically query Dobot joint angles, convert angles to radians, and publish `sensor_msgs/msg/JointState`.

1. **Subscriber Callbacks**: Listen for target coordinates (`geometry_msgs/msg/Point`), parse them into Dobot-specific serial packets, and write them to the USB serial port.

- **Control Space Modes**

	- **Joint Space**: Explicitly command individual motor angles (e.g., Joint 1 = 30°, Joint 2 = 45°).

	- **Cartesian Space**: Command target coordinates relative to base (e.g., $ X=200\text{mm}, Y=0\text{mm}, Z=50\text{mm} $). The Dobot’s internal firmware solves the inverse kinematics for you.

---

### 🛠️ Hands-on Lab

- Install serial package resources:

	
```bash
pip install pyserial
```

- Connect the Dobot Magician using a USB cable.

- Run a Python test script checking serial handshakes and basic connection confirmation to verify the link.

---

### 📝 Assignment

- **Task**: Write a ROS2 node `dobot_serial_driver.py` that connects to the Dobot. The node must subscribe to `/joint_commands` (`std_msgs/msg/Float32MultiArray`) and execute motor writes while publishing current joint states to `/joint_states` at 20Hz.

