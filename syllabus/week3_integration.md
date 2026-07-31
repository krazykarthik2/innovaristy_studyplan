# Week 3: Robot Integration (Days 11–15)

Welcome to Week 3! This week bridges the gap between software development and physical robotics. You will construct URDF models, interface with 3D simulation engines, control physical manipulators, and design control stacks for 6-DoF surgical systems.

---

## Day 11: URDF & Robot Description

### 📖 Detailed Structured Syllabus
1. **Introduction to URDF (Unified Robot Description Format)**
   * XML representation of robot kinematics, dynamics, visual representations, and collision geometries.
   * Basic building blocks: `<robot>`, `<link>`, `<joint>`.
2. **Robot Links: Physical Parameters**
   * Visual geometries: Box, Cylinder, Sphere, or external mesh file types (STL/DAE).
   * Collision geometries: Simplified bounding boxes to reduce computation in contact solvers.
   * Inertial properties: Mass, center of gravity (origin), and rotational inertia tensors ($I_{xx}, I_{xy}$, etc.).
3. **Robot Joints: Kinematic Links**
   * Joint types: revolute (rotational with bounds), continuous (unbounded rotation), prismatic (linear slider), fixed, floating.
   * Joint limits: lower/upper limits (radians/meters), velocity limits, effort limits.
   * Dynamic variables: friction, damping coefficients.
4. **URDF Verification Tools**
   * Parsing URDF files: `urdf_parser_py`, `check_urdf`.
   * Rendering URDF trees: `urdf_to_graphiz`.

### 🛠️ Hands-on Labs & Implementations
* **Lab 11.1: Construct a Simple Multi-Link URDF**
  * Create a raw XML document `simple_arm.urdf` representing a two-link planar arm with revolute joints.
* **Lab 11.2: Verify URDF Kinematic Structures**
  * Parse your URDF configuration through the console analyzer:
    ```bash
    check_urdf simple_arm.urdf
    ```

### 📝 Assignment
* **Task**: Create a detailed URDF model for a 4-DoF Robot Arm (`4dof_robot.urdf`). Include accurate visual parameters (cylinders for joints, blocks for arm linkages), defined collision volumes, standard mass parameters, joint limits, and coordinate system origins. Save the XML description and visual schematic in the workspace.

---

## Day 12: RViz, Gazebo & MoveIt2

### 📖 Detailed Structured Syllabus
1. **RViz (ROS Visualization Tool)**
   * Why RViz? 3D visualization tool for sensor states, map generation, transforms, and planned trajectories.
   * Adding display panels: RobotModel, TF, LaserScan, InteractiveMarkers.
   * Configuring the fixed frame context.
2. **Gazebo Physics Simulator**
   * Why Gazebo? Full physical simulation engine (ODE/Bullet physics, sensor feedback, force/torque solvers).
   * Differences between RViz and Gazebo (visualization vs. simulation).
   * Creating Gazebo worlds, loading URDF files, writing SDF configurations.
   * Actuator plugins: `gazebo_ros2_control` configuration.
3. **MoveIt2 (Motion Planning Framework)**
   * Introduction to MoveIt2: Kinematics solvers (IKFast, KDL), collision detection, trajectory generation.
   * Setup Assistant configuration workflow.
   * Planning groups, robot states, motion controllers.

### 🛠️ Hands-on Labs & Implementations
* **Lab 12.1: Visualize URDF in RViz**
  * Create a launch script that starts the `robot_state_publisher` and opens RViz displaying your 4-DoF robot model.
* **Lab 12.2: Launch Robot in Gazebo**
  * Add inertial tags to your URDF, configure Gazebo plugins, and launch the robot in a mock physics workspace.

### 📝 Assignment
* **Task**: Develop a MoveIt2 configuration package for the 4-DoF robot arm using the MoveIt Setup Assistant. Setup planning groups (e.g., "arm" and "hand/end_effector"), specify IK solvers, and write a script to command the virtual robot to run smooth trajectory plans between home and target Cartesian points.

---

## Day 13: Dobot Magician ROS2 Integration

### 📖 Detailed Structured Syllabus
1. **Dobot Magician Hardware Overview**
   * Kinematic layout: 4-DoF desktop manipulator (joint controls: base, rear arm, forearm, end-effector rotation).
   * Control board specs, payload restrictions, communication protocols.
2. **Serial Communication Interfaces**
   * Interfacing via USB: `/dev/ttyUSB0` or `/dev/ttyACM0`.
   * Protocol analysis: framing, command types, checksum structures.
   * Linux USB permissions: configuration of `dialout` groups (`sudo usermod -a -G dialout $USER`).
3. **Joint States Publishing**
   * Integrating sensory feedback from the Dobot.
   * Creating a ROS2 node that queries joint positions from the Dobot board via serial and broadcasts `sensor_msgs/msg/JointState`.
4. **Motion Command Execution**
   * Point-to-Point (PTP) motion vs. Continuous Path (CP) motion.
   * Translating ROS2 commands (Cartesian target coordinates / joint angles) into raw serial control API requests.
5. **Control Modes**
   * Joint Motion Space vs. Cartesian Coordinate Space.
   * Utilizing end-effectors: Suction cup control, gripper triggers, write/draw pen mechanisms.

### 🛠️ Hands-on Labs & Implementations
* **Lab 13.1: Install Drivers & Connect to Hardware**
  * Configure serial permissions on the Ubuntu host machine.
  * Connect the Dobot Magician via USB and establish basic serial handshake validation.
* **Lab 13.2: Write Joint Position Query Node**
  * Implement a ROS2 Python node that reads serial telemetry streams from the physical controller and logs current coordinates.

### 📝 Assignment
* **Task**: Create an integrated pick-and-place application (`dobot_pick_and_place`). Write a ROS2 controller node that accepts coordinates for two targets (Pick Location, Drop Location). The node must command the Dobot Magician to move to the pick point, activate the suction cup end-effector via serial write commands, translate to the drop location, deactivate suction, and return to a default home position.

---

## Day 14: 6-DoF Surgical Robot Architecture

### 📖 Detailed Structured Syllabus
1. **Introduction to the Taurean 6-DoF Surgical Robot**
   * System overview: Mechanical footprint, degrees of freedom, surgical precision requirements.
   * Layered safety requirements: Hardware safety relays, software checks, software-defined limits.
2. **Modular Surgical Robot Software Stack**
   * **Robot Layers**: High-level application controller down to hardware abstractors.
   * **Hardware Interface**: Mapping ROS2 control commands to physical motor drivers (EtherCAT/CANopen).
   * **Controller Manager**: Loading, spawning, and managing active controllers (Joint Trajectory Controller, Joint State Broadcaster).
   * **Safety Layer**: Collision check loops, kinematic bounds monitoring, emergency-stop (E-Stop) interfaces.
   * **Motion Planner**: Generates collision-free tool paths (MoveIt2/OMPL).
   * **Forward & Inverse Kinematics**: Computes Cartesian pose from joint angles (FK) and joint angles from desired pose (IK).
   * **State Machine**: Monitors robot states (Idle, Calibrating, Safe Mode, Active Surgical Mode, Emergency Shutdown).
   * **Diagnostics**: Active monitoring of motor temperatures, encoder values, and latency flags.

### 🛠️ Hands-on Labs & Implementations
* **Lab 14.1: Study Existing Software Architectures**
  * Read, analyze, and map out the signal data flow diagram of a standard multi-joint surgical manipulator controller stack (using ROS2 Control).
* **Lab 14.2: Implement basic Kinematic Solver Nodes**
  * Write a Python node that calculates the forward kinematics of a 3-link planar manipulator using custom trigonometric configurations.

### 📝 Assignment
* **Task**: Design a modular software module architecture for the Taurean 6-DoF Surgical Robot. Document this architecture in a markdown file (`surgical_robot_design.md`). Include a system component diagram (using Mermaid syntax) representing the interaction between:
  1. User Dashboard / Teleoperation input.
  2. Safety validation layer.
  3. Joint trajectory controllers.
  4. Real-time controller manager.
  5. Low-level CAN/EtherCAT driver interface.
  Describe the function, inputs, outputs, and safety requirements of each block.

---

## Day 15: Final Research Project

### 🎯 Objective
Apply the entire curriculum's concepts to develop a fully integrated ROS2 workspace showcasing simulation, transformation, path planning, and physical device control capabilities.

### 📋 Deliverables
1. **Robot Workspace (`final_research_ws/`)**
   * **`robot_description` Package**: Containing the URDF model of the robot with full visual/collision properties.
   * **`robot_control` Package**: Node implementations for trajectory execution (Action server), TF broadcasting, and motor control drivers.
   * **`robot_launch` Package**: Launch files configuring the complete simulation/hardware setup in one terminal call.
2. **GitHub Repository**
   * Complete source code, clean history, and detailed `README.md` setup guide.
3. **Final Presentation / Live Demonstration**
   * **System Setup**: ROS2 installation verification.
   * **Simulation Run**: Robot URDF visualization in RViz and kinematics control in Gazebo.
   * **Interfacing**: Physical/Mock Dobot Magician control demonstration.
   * **Motion Planning**: Safe path execution via joint controllers.
   * **Architecture Review**: Structural breakdown and future integration path for the 6-DoF Taurean surgical stack.

### 📝 Assignment
* **Task**: Complete the Final Research Project. Pack all packages into a clean workspace, write a detailed deployment guide (`final_project_docs.md`), compile the codebase, and verify package execution without warnings.

---

*For detailed architectural specifications, navigate to [**Expected Final Software Stack Understanding**](architecture_and_project.md).*
