# Day 07: ROS2 Services & Config Parameters

### 📌 TL;DR
Understand request-reply interactions (Services), configure global parameters for nodes, and load configuration settings using YAML files.

---

### 📖 The Core Topics

* **ROS2 Services (.srv): Request-Reply**
  * One-way topics aren't suited for transaction operations (e.g., commanding a camera to take a photo).
  * Services provide synchronous or asynchronous request-reply communication.
  * A client sends a **Request**, the server processes it and sends back a **Response**.
  * Command-line inspection:
    * `ros2 service list`: Active service channels.
    * `ros2 service type /reset`: Show interface format.
    * `ros2 service call /reset std_srvs/srv/Empty`: Trigger call directly from the terminal.
* **Writing custom Service Definitions**
  * Created inside an interface package's `srv/` directory:
    ```
    # SetState.srv
    # Request
    string desired_state
    float64 timeout
    ---
    # Response
    bool success
    string message
    ```
* **ROS2 Parameters (Node Configs)**
  * Configuration values stored inside nodes (e.g., maximum velocity limits, serial port names).
  * Declaring parameters in Python:
    ```python
    self.declare_parameter('max_speed', 0.5)
    ```
  * Accessing parameters at runtime:
    ```python
    current_limit = self.get_parameter('max_speed').value
    ```
  * Command-line interface parameters:
    * `ros2 param list`: Lists parameters of all nodes.
    * `ros2 param get /my_node max_speed`: View value.
    * `ros2 param set /my_node max_speed 1.2`: Change value on the fly.
* **Loading Parameters from YAML files**
  * Store system parameters in a single configuration file:
    ```yaml
    # config.yaml
    /my_node:
      ros__parameters:
        max_speed: 1.5
        port: "/dev/ttyUSB0"
    ```
  * Launch nodes with the parameters file:
    ```bash
    ros2 run my_pkg my_node_exe --ros-args --params-file config.yaml
    ```

---

### 🛠️ Hands-on Lab
* Implement a Python service server node that changes a motor state parameter (e.g., "STOPPED", "RUNNING") when receiving a request.
* Test calling this service server using CLI commands.

---

### 📝 Assignment
* **Task**: Create a node `safety_monitor` with parameters for safety range limits and battery threshold values. Add a service interface `/reconfigure` allowing users to change these parameters via terminal calls. Dump the updated parameter state to a local file using `ros2 param dump`.
