# Day 09: ROS2 Launch Systems & Multi-Node Coordination

### 📌 TL;DR
Understand why we use launch files, how to write launch scripts in Python, isolate identical nodes using namespaces, and remap topic destinations dynamically at startup.

---

### 📖 The Core Topics

* **Why Launch Files?**
  * Starting nodes one-by-one in separate terminal windows is tedious.
  * Launch files allow you to start multiple nodes, load configuration files, define namespaces, and set environment variables using a single command.
  * Launch command: `ros2 launch <package_name> <launch_file_name>`.
* **Writing Python Launch Files**
  * ROS2 launch files are written in Python for dynamic configurability:
    ```python
    from launch import LaunchDescription
    from launch_ros.actions import Node

    def generate_launch_description():
        return LaunchDescription([
            Node(
                package='my_package',
                executable='talker_node',
                name='custom_talker',
                output='screen'
            ),
            Node(
                package='my_package',
                executable='listener_node',
                name='custom_listener',
                output='screen'
            )
        ])
    ```
* **Isolating Environments: Namespaces**
  * Launching identical nodes without conflicts by scoping their topics:
    * Node A runs in namespace `/robot_1` (publishes to `/robot_1/sensor_data`).
    * Node B runs in namespace `/robot_2` (publishes to `/robot_2/sensor_data`).
* **Dynamic Topic Routing: Remapping**
  * Change a node's topic name without rewriting its source code:
    ```python
    Node(
        package='telemetry',
        executable='publisher_node',
        remappings=[('/old_chatter', '/new_telemetry_stream')]
    )
    ```
* **Loading Parameter files (YAML) in Launch**
  * Pass parameter configurations directly to nodes at launch:
    ```python
    Node(
        package='my_pkg',
        executable='my_node',
        parameters=[LaunchConfiguration('params_file')]
    )
    ```

---

### 🛠️ Hands-on Lab
* Create a launch package containing a `system_launch.py` script.
* Configure it to spawn three nodes simultaneously: two `Talker` nodes in separate namespaces (`/arm_left` and `/arm_right`) and one global `Listener` node mapping incoming values.

---

### 📝 Assignment
* **Task**: Develop a "One-Click Robot Startup" file (`robot_startup_launch.py`). This script must configure namespaces, map topic paths dynamically, load logging variables, read parameters from a local YAML file, and output confirmation alerts.
