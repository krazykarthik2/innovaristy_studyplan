# Day 12: RViz, Gazebo & MoveIt2 Motion Planning

### 📌 TL;DR
Bring your robot description (URDF) to life. Use RViz to visualize coordinates/sensors, Gazebo to run physics-based simulation models, and MoveIt2 to plan collision-free paths.

---

### 📖 The Core Topics

* **RViz (Visualization Tool)**
  * **What it does**: Displays *what the robot thinks is happening*.
  * Shows coordinate frames (TF), sensor inputs (LiDAR point clouds, camera feeds), and planned trajectory paths.
  * *Important*: RViz is NOT a physics simulator. If you drag the robot in RViz, it won't fall down due to gravity.
* **Gazebo (Physics Simulator)**
  * **What it does**: Simulates *the real world*.
  * Calculates gravity, friction, mass, and collision contacts.
  * Standard configuration: Connects to ROS2 using the `gazebo_ros2_control` plugin to read actuator target commands.
* **MoveIt2 (The Motion Planner)**
  * **What it does**: Solves the complex math of how to move a multi-joint arm from Point A to Point B without hitting obstacles.
  * Solves Inverse Kinematics (IK) automatically.
  * Includes collision checking: Uses your URDF's `<collision>` tags to ensure the arm links do not collide with themselves or surrounding tables/objects.
  * **MoveIt Setup Assistant**: A graphical tool used to configure planning groups, end-effectors, default poses, and collision vectors:
    ```bash
    ros2 launch moveit_setup_assistant setup_assistant.launch.py
    ```

---

### 🛠️ Hands-on Lab
* Launch the `robot_state_publisher` to publish your URDF structures.
* Open RViz:
  ```bash
  rviz2
  ```
* Add the `RobotModel` display panel and set the fixed frame parameter to `base_link` to view your 3D robot model.

---

### 📝 Assignment
* **Task**: Generate a MoveIt2 configuration package for your 4-DoF Robot Arm (`4dof_moveit_config`). Define the main arm planning group, configure kinematic solver interfaces (KDL/IKFast), and verify self-collision matrices.
