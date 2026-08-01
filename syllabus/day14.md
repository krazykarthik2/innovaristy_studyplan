---
notion-url: https://www.notion.so/3aec4db880d981bda81adc69038f04c3
title: day14
date: '2026-07-31 20:19:00.000'
from_notion: https://app.notion.com/p/day14-3aec4db880d981bda81adc69038f04c3
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Day 14: 6-DoF Taurean Surgical Robot Stack (Practical)

### 📌 TL;DR

---

### 🛠️ Practical Architecture Components


```mermaid
graph LR
    User[GUI Dashboard] -->|Reconfigure Service| Safety[Safety Controller Node]
    User -->|Target Goal| Motion[MoveIt2 Planning Node]
    Motion -->|Trajectory Joint Points| Manager[Controller Manager]
    Manager -->|Joint Positions| Driver[Low-level EtherCAT Node]
    Driver -->|Actuator Signals| Motors[Physical Motors]
    Motors -->|Encoder Telemetry| Driver
    Driver -->|Joint Telemetry| Broadcaster[Joint State Broadcaster]
    Broadcaster -->|/joint_states| Safety
    Safety -->|Validated Telemetry| User
```

[//]: # (heading_4 is not supported)

- **Goal**: Write a C++ node `ethercat_motor_driver` communicating directly with motor controllers (like Maxon EPOS4) over a CAN/EtherCAT fieldbus interface.

- **Frequency**: Must run on a real-time thread loop at $ 1000\text{ Hz} $ (1ms tick rates) using `PREEMPT_RT` patch schedules.

- **Payload**: Receives target position values, writes to control word registries, and reads encoder counts.

[//]: # (heading_4 is not supported)

- Use the ROS2 Control framework (`ros2_control`).

- Create a config file `controllers.yaml` to configure controller containers:

	- **Joint State Broadcaster**: Publishes joint angles to `/joint_states` for RViz.

	- **Joint Trajectory Controller**: Receives spline target trajectories from MoveIt2 and performs real-time interpolation to generate command signals.

[//]: # (heading_4 is not supported)

- **Goal**: Write a Python node `safety_supervisor` checking robot kinematics state variables.

- **Tasks**:

	- Subscribe to `/joint_states`.

	- Calculate Forward Kinematics (FK) to find 3D cartesian tools position.

	- If the tool moves out of bounds (virtual safety box), or if velocities exceed thresholds:

- Trigger a service request `/estop` to cut motor power immediately.

---

### 🛠️ Hands-on Implementation Exercise

- Create the configuration package structure:

	
```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_cmake taurean_control_stack --dependencies rclcpp ros2_control controller_manager
```

- Define custom service interfaces inside `taurean_interfaces/srv/`:

	- `EStop.srv` (contains a trigger status input and confirmation output).

	- `SetSafetyLimits.srv` (defines Cartesian bounding box dimensions).

---

### 📝 Assignment

- **Task**: Design the modular software architecture document (`surgical_robot_design.md`) outlining node responsibilities, communication interfaces (topics, service schemas, actions), low-level parameters, and specific safety rules.

