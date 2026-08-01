---
notion-url: https://www.notion.so/3aec4db880d98129985ade9d0eb1a091
title: week2_development
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/week2_development-3aec4db880d98129985ade9d0eb1a091
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Week 2: ROS2 Development (Days 6–10)

---

## Day 6: ROS2 Packages

### 📖 Detailed Structured Syllabus

1. **ROS2 Package Architecture**

	- What is a package? The fundamental unit of build and release.

	- C++ (`ament_cmake`) package layout vs. Python (`ament_python`) package layout.

	- Core files: `package.xml` (metadata, dependencies, build tool settings) and `CMakeLists.txt` or `setup.py` / `setup.cfg`.

1. **C++ Package Anatomy (**`**ament_cmake**`**)**

	- Exploring `CMakeLists.txt`: `find_package()`, `add_executable()`, `ament_target_dependencies()`, `install()`.

	- Standard directories: `/src`, `/include/<package_name>/`.

1. **Python Package Anatomy (**`**ament_python**`**)**

	- Understanding `setup.py`: `setup()`, script entry points, installation files.

	- Standard directories: `/<package_name>`, `/resource`.

1. **Dependency Management**

	- Declaring dependencies in `package.xml`: `<depend>`, `<build_depend>`, `<exec_depend>`, `<test_depend>`.

	- Resolving dependencies using `rosdep`: `rosdep install --from-paths src --ignore-src -r -y`.

1. **Colcon Compilation Options**

	- Workspace directories: `/src`, `/build`, `/install`, `/log`.

	- Advanced building commands:

		- `colcon build --packages-select <pkg_name>`

		- `colcon build --symlink-install` (crucial for Python development to link files rather than copying on build)

		- `colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release`

### 🛠️ Hands-on Labs & Implementations

- **Lab 6.1: Create a Python Package**

	- Generate a Python-based ROS2 package:

		
```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python my_py_package --dependencies rclpy std_msgs
```

- **Lab 6.2: Create a C++ Package**

	- Generate a C++ based ROS2 package:

		
```bash
ros2 pkg create --build-type ament_cmake my_cpp_package --dependencies rclcpp std_msgs
```

### 📝 Assignment

- **Task**: Build a fully structured workspace called `surgical_robot_ws`. Inside `src/`, create three packages: `surgical_teleop` (Python), `surgical_hardware_interface` (C++), and `surgical_interfaces` (for custom messages and services). Ensure all `package.xml` and configuration files (`CMakeLists.txt`/`setup.py`) link dependencies correctly. Document the workspace layout and build validation commands in a file named `workspace_structure.md`.

---

## Day 7: Services & Parameters

### 📖 Detailed Structured Syllabus

1. **ROS2 Services (srv)**

	- Request/Response communication pattern (synchronous/asynchronous blocking calls).

	- Service definitions (.srv file structure: Request fields, separator `--`, Response fields).

	- Creating a service server: `create_service(srv_type, service_name, callback)`.

	- Creating a service client: `create_client(srv_type, service_name)` and calling `call_async()`.

1. **ROS2 Parameters**

	- Global configuration system for individual nodes.

	- Declaring parameters: `self.declare_parameter('param_name', default_value)`.

	- Reading parameters: `self.get_parameter('param_name').value`.

	- Modifying parameters on the fly using CLI tools.

1. **YAML Configuration Files**

	- Structure of a ROS2 parameter YAML file.

	- Hierarchical layout under node names.

	- Passing YAML parameters directly to nodes at runtime: `ros2 run <pkg> <executable> --ros-args --params-file <path_to_yaml>`.

1. **Command Line Tools**

	- Services: `ros2 service list`, `ros2 service call <service_name> <srv_type> <args>`, `ros2 service type <service_name>`.

	- Parameters: `ros2 param list`, `ros2 param get <node> <param>`, `ros2 param set <node> <param> <value>`, `ros2 param dump <node>`.

### 🛠️ Hands-on Labs & Implementations

- **Lab 7.1: Build Service Server & Client**

	- Implement a Python node that hosts a `/reset_robot_position` service which receives coordinate inputs and returns a status success boolean.

	- Create a client node that connects to `/reset_robot_position` and triggers requests periodically.

- **Lab 7.2: Implement Parameters in Nodes**

	- Create a node that reads a configurable logging level (e.g., Debug, Info, Warning) and execution frequency parameter from a YAML file.

### 📝 Assignment

- **Task**: Develop a configurable robot parameter controller. Create a node named `robot_parameter_server` that hosts parameters for joint safety limits, maximum velocity, and hardware mode. Add a service interface `/update_safety_profile` that accepts a profile name (e.g., “SAFE”, “HIGH_SPEED”) and modifies the underlying parameter values dynamically.

---

## Day 8: Actions

### 📖 Detailed Structured Syllabus

1. **ROS2 Actions (action)**

	- Asynchronous, long-running processes (Goal request, Feedback updates, Result response).

	- Action definitions (.action file layout: Goal section, `--`, Result section, `--`, Feedback section).

	- Difference between Topics, Services, and Actions: When to use which?

1. **Action Server Mechanics**

	- Instantiating action servers: `rclpy.action.ActionServer`.

	- Handlers for accepting/rejecting goals: `goal_callback`, `handle_accepted_callback`.

	- Running non-blocking execution loops using secondary threads/executors to stream feedback.

1. **Action Client Mechanics**

	- Instantiating action clients: `rclpy.action.ActionClient`.

	- Sending goals asynchronously: `send_goal_async()`.

	- Getting feedback callbacks: `feedback_callback(feedback_msg)`.

	- Fetching final execution results: `get_result_callback()`.

1. **CLI Interaction**

	- Finding active actions: `ros2 action list`.

	- Triggering action goals: `ros2 action send_goal <action_name> <action_type> <goal_payload> --feedback`.

### 🛠️ Hands-on Labs & Implementations

- **Lab 8.1: Write custom action definition**

	- Write a custom Action description `NavigateToPose.action`.

- **Lab 8.2: Implement Python Action Server**

	- Create an action server that accepts a target location, simulates navigation trajectory step-by-step, publishes percent completion feedback, and returns success on completion.

### 📝 Assignment

- **Task**: Build a complete Robot Movement Simulator (`robot_movement_action`). The action server accepts a list of target joint angles (goal), simulates joint interpolation over time, publishes feedback showing current joint angles and distance-to-target every 100ms, and yields a final status result containing execution duration and average deviation.

---

## Day 9: Launch Files

### 📖 Detailed Structured Syllabus

1. **Role of Launch Files in Robotics**

	- Why launch files? Single command system activation for multiple nodes, configurations, and parameter scripts.

	- Legacy XML vs. Modern Python-based launch configurations.

1. **ROS2 Launch Framework (Python API)**

	- Core modules: `launch`, `launch_ros`.

	- Declarative execution structure: `GenerateLaunchDescription()`.

	- Common actions: `Node`, `ExecuteProcess`, `IncludeLaunchDescription`, `RegisterEventHandler`.

1. **Namespacing & Remapping**

	- Launching identical nodes in isolated domains using `namespace`.

	- Re-routing topic communication at start using `remappings=[('old_topic', 'new_topic')]`.

1. **Logging Infrastructure**

	- Customizing console log formats.

	- Redirecting outputs: `output='screen'` vs. logging to `/home/user/.ros/log`.

	- Controlling verbosity: `arguments=['--ros-args', '--log-level', 'DEBUG']`.

1. **Launch Arguments & Conditions**

	- Defining system variables: `DeclareLaunchArgument`.

	- Dynamic evaluation at launch: `LaunchConfiguration`.

	- Conditional execution: `IfCondition`, `UnlessCondition`.

### 🛠️ Hands-on Labs & Implementations

- **Lab 9.1: Build Multi-Node Launch File**

	- Write a python script `system_launch.py` to start three custom nodes simultaneously: `Talker`, `Listener`, and the `ParameterServer`.

- **Lab 9.2: Launch Node Namespacing**

	- Launch two distinct instances of `Talker` under namespace environments `/robot_left` and `/robot_right`.

### 📝 Assignment

- **Task**: Create a “One-Click Robot Startup” launch file (`robot_startup_launch.py`). This script must declare input arguments for selecting hardware simulation modes, spawn nodes with correct namespaces and logging outputs, load parameters from a YAML file, and execute a CLI terminal script displaying diagnostic outputs when the initialization finishes successfully.

---

## Day 10: TF2 & Robot Frames

### 📖 Detailed Structured Syllabus

1. **Spatial Transformations in Robotics**

	- Coordinate frames: Base link, tool frames, world frame.

	- Vector space transformation, rotation matrices, quaternions vs. Euler angles.

1. **The TF2 Library**

	- What is tf2? Distributed tree tracking translation and rotation between coordinate frames over time.

	- Tree structures: Rules of TF (single parent constraint).

1. **Static transforms vs. Dynamic transforms**

	- Static transforms: Fixed relations (e.g., base link to sensor bracket). Published via `StaticTransformBroadcaster`.

	- Dynamic transforms: Changing relations (e.g., base link to moving arm joint). Published via `TransformBroadcaster`.

1. **Broadcasting TF Data**

	- Filling `TransformStamped` messages.

	- Configuring timestamp headers, parent frames, and child frames.

1. **Listening to TF Data**

	- Instantiating `TransformListener` and `Buffer`.

	- Looking up transformations: `buffer.lookup_transform(target_frame, source_frame, time)`.

1. **TF Visualization & Debugging Tools**

	- Command Line: `ros2 run tf2_ros tf2_echo <parent_frame> <child_frame>`, `ros2 run tf2_tools view_frames`.

	- Graphical: RViz visualization (TF display plugin).

### 🛠️ Hands-on Labs & Implementations

- **Lab 10.1: Broadcast Static Transforms**

	- Implement a static tf broadcaster node that defines the position of a sensor frame `camera_link` relative to `base_link` (translation: [0.1, 0, 0.5], rotation: 0).

- **Lab 10.2: Broadcast Dynamic Transformations**

	- Write a dynamic broadcaster that reads joint position variables and publishes a spinning joint transform frame `/joint_1` relative to `base_link`.

### 📝 Assignment

- **Task**: Create a Transform Tree for a 4-DoF Robot. Implement a node named `robot_tf_broadcaster` that calculates and broadcasts transformations for `base_link` -> `link_1` -> `link_2` -> `link_3` -> `end_effector`. Update the transforms dynamically using a time step parameter. Verify the tree structure using `ros2 run tf2_tools view_frames` and save the generated PDF tree representation (`frames.pdf`) in the workspace.

---

