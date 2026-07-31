# Day 06: ROS2 Package Design (CMake & Python)

### 📌 TL;DR
Understand package architecture, dependencies, build settings (`setup.py` and `CMakeLists.txt`), and how to compile separate C++ and Python node containers.

---

### 📖 The Core Topics

* **ROS2 Package Concept**
  * You cannot compile loose Python/C++ scripts directly in ROS2. They must live in a **Package**.
  * Packages allow you to bundle code, launch files, configurations, and custom message types.
* **Creating a Python-based Package (`ament_python`)**
  * Generate command:
    ```bash
    ros2 pkg create --build-type ament_python <package_name> --dependencies rclpy std_msgs
    ```
  * Folder structure:
    * `package.xml`: Holds package metadata (name, description, author, dependencies).
    * `setup.py`: Lists entry points (which Python scripts become runnable commands).
    * `setup.cfg`: Internal configuration specifying target executable destinations.
    * `<package_name>/`: Directory containing your actual Python code files.
* **Creating a C++ based Package (`ament_cmake`)**
  * Generate command:
    ```bash
    ros2 pkg create --build-type ament_cmake <package_name> --dependencies rclcpp std_msgs
    ```
  * Folder structure:
    * `package.xml`: Holds package metadata and C++ dependencies.
    * `CMakeLists.txt`: Build script for the compiler (defines executable outputs and includes).
    * `src/`: Directory containing your `.cpp` source files.
    * `include/`: Header files (`.hpp`).
* **Managing package.xml Dependencies**
  * Ensure dependencies are correctly declared in your `package.xml` so the package manager knows what to install:
    * `<depend>rclpy</depend>`
    * `<depend>std_msgs</depend>`
  * Run dependency resolver command to automatically verify and install missing libs:
    ```bash
    rosdep install --from-paths src --ignore-src -y -r
    ```
* **Defining Entry Points in setup.py**
  * In a Python package, add your nodes to the `entry_points` list in `setup.py`:
    ```python
    entry_points={
        'console_scripts': [
            'my_publisher = my_package.publisher_script:main',
            'my_subscriber = my_package.subscriber_script:main',
        ],
    },
    ```

---

### 🛠️ Hands-on Lab
* Navigate to your workspace directory `~/ros2_ws/src`.
* Create a Python package `telemetry_core` using the CLI template tools.
* Add your Day 5 publisher/subscriber scripts to `telemetry_core/telemetry_core/`.
* Configure the entry points in `setup.py`, run `colcon build --symlink-install` from the workspace root, source your environment, and test your new node commands.

---

### 📝 Assignment
* **Task**: Create a C++ package named `diagnostics_cpp` containing a single C++ node that prints memory configurations to terminal. Configure the `CMakeLists.txt` block, compile the project, and document the configuration layout in `workspace_structure.md`.
