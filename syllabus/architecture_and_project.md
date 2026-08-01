---
notion-url: https://www.notion.so/3aec4db880d981a4bc4be46e28b55ad1
title: architecture_and_project
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/architecture_and_project-3aec4db880d981a4bc4be46e28b55ad1
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Expected Final Software Stack Architecture

---

## 🏗️ Layered Software Architecture


```mermaid
graph TD
    %% Define layers
    subgraph Application_Layer [Application Layer]
        A1["Surgical Procedure Applications (GUI)"]
        A2["Teleoperation Interface (Master Console)"]
        A3["User Dashboard (Diagnostics)"]
    end

    subgraph Motion_Planning_Layer [Motion Planning Layer]
        M1["MoveIt2 Planning Pipeline"]
        M2["Trajectory Generator (Joint Space)"]
        M3["Cartesian Path Planner"]
    end

    subgraph Control_Layer [Control Layer]
        C1["Joint Trajectory Controller"]
        C2["Velocity / Position Controllers"]
        C3["PID Control Loops"]
    end

    subgraph Abstraction_Layer [Robot Abstraction Layer]
        R1["Hardware Component Interfaces (ros2_control)"]
        R2["Sensor Interface (IMU, Force/Torque)"]
        R3["Encoder & Driver Interfaces"]
    end

    subgraph Middleware_Layer [ROS2 Middleware - DDS]
        D1["Topics (Telemetry)"]
        D2["Services (System Requests)"]
        D3["Actions (Kinematic Actions)"]
        D4["TF2 Frame Transforms"]
        D5["Parameters (Safety Limits)"]
    end

    subgraph OS_Layer [Linux Ubuntu OS]
        O1["Real-Time Kernel (PREEMPT_RT)"]
        O2["Systemd Services & Network Stack"]
    end

    subgraph Hardware_Layer [Physical Hardware]
        H1["6-DoF Taurean Surgical Robot / 4-DoF Dobot"]
        H2["Motors, Drivers, Sensors, CAN/EtherCAT bus"]
    end

    %% Data Flow / Connections
    A2 -->|Target Cart/Joint Poses| M1
    M1 -->|Collision-Free Path| M2
    M2 -->|Trajectory Waypoints| C1
    C1 -->|Target States (Effort/Vel)| R1
    R1 -->|Low-Level Commands| H2
    H2 -->|Encoder Feedback| R3
    R3 -->|Joint States| D1
    D1 -->|State Telemetry| A3
    D4 -.->|Transforms| M1
    D5 -.->|Safety Configuration| C1
    O1 -->|Process Scheduling| C1
    O2 -->|Network/SSH| A2
```

---

## 📂 Detailed Layer Analysis

### 1. Application Layer

- **Surgical Procedure Applications**: Custom high-level interfaces designed to guide surgeons through automated calibration routines, instrument swaps, and preoperative workflows.

- **Teleoperation Interface**: Reads outputs from input devices (like haptic master controllers or joysticks) and maps spatial commands directly to end-effector coordinates.

- **User Dashboard**: Real-time visualization interface mapping system state variables, temperature metrics, error logs, and streaming video feeds.

### 2. Motion Planning Layer

- **MoveIt2**: Integrates kinematic solvers (e.g., KDL or custom analytical IK) to solve inverse kinematics and generate smooth joint paths.

- **Trajectory Planning**: Formulates time-parameterized paths using algorithms like spline interpolation to ensure velocity and acceleration bounds are respected.

- **Cartesian Planner**: Plans straight-line or curvilinear paths in 3D space, which is critical for surgical procedures like incisions or insertion paths.

### 3. Control Layer

- **Joint Controller**: Ensures the physical motor positions match the target joints command.

- **PID Loops**: Implements Proportional-Integral-Derivative control equations running at high frequencies (typically 1 kHz) to minimize trajectory tracking errors.

- **Safety Limits**: Overrides control commands if current readings exceed maximum bounds, ensuring physical limits are never broken.

### 4. Robot Abstraction Layer

- **ros2_control Hardware Interface**: A standardized C++ abstraction layer providing consistent interfaces for reading state variables and writing command variables across different actuators.

- **Sensor Interface**: Normalizes signals from external sensors (e.g., force/torque sensors, IMUs, camera feeds).

- **Driver Interface**: Direct bus controllers transmitting commands over low-latency protocols like CANopen or EtherCAT.

### 5. ROS2 Middleware Layer

- **Topics, Services, Actions**: The transport structure enabling modular packages to communicate asynchronously.

- **TF2 (Transforms)**: Keeps track of coordinates between coordinate systems (e.g., base coordinate to patient coordinate to tool tip coordinate).

- **Parameters**: Node configurations defining variables like safety margins and tool limits dynamically.

### 6. Linux Ubuntu Layer

- **Ubuntu OS**: The baseline platform managing files, resources, and development compilation environments.

- **Real-Time Kernel (**`**PREEMPT_RT**`**)**: Ensures execution schedules for control loops are completely deterministic, preventing jitter or lag in motor control commands.

### 7. Physical Hardware

- **Manipulator Arms**: The mechanical bodies of the 4-DoF Dobot Magician (learning system) and the 6-DoF Taurean Surgical Robot (production target).

- **Actuators & Sensors**: Maxon motors, absolute encoders, force sensors, and bus systems.

