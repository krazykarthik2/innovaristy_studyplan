---
notion-url: https://www.notion.so/3aec4db880d9817eacb6c87162b16e36
title: day07
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/day07-3aec4db880d9817eacb6c87162b16e36
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Day 07: ROS2 Services & Config Parameters

### 📌 TL;DR

---

### 📖 The Core Topics

- **ROS2 Services (.srv): Request-Reply**

	- One-way topics aren’t suited for transaction operations (e.g., commanding a camera to take a photo).

	- Services provide synchronous or asynchronous request-reply communication.

	- A client sends a **Request**, the server processes it and sends back a **Response**.

	- Command-line inspection:

- `ros2 service list`: Active service channels.

- `ros2 service type /reset`: Show interface format.

- `ros2 service call /reset std_srvs/srv/Empty`: Trigger call directly from the terminal.

- **Writing Custom Service Definitions (.srv Files)**

	- **The .srv File Format (Blueprint)**:

- Created inside your ROS2 package’s `srv/` directory (e.g., `SetSpeed.srv`).

- The file format requires three dashes `--` to separate request fields from response fields:

	
```protobuf
# --- REQUEST (Data sent by the client) ---
float64 speed
string direction
---
# --- RESPONSE (Data sent back by the server) ---
bool success
string message
```

- **How to Write an Active Service (Python Code)**:

	- Minimal, complete Python node using `rclpy` to host an active service `/set_robot_speed` using the interface `my_custom_package/srv/SetSpeed`:

		
```python
import rclpy
from rclpy.node import Node

# 1. Import the service interface generated from your .srv file
from my_custom_package.srv import SetSpeed

class SpeedServiceNode(Node):
    def __init__(self):
        super().__init__('speed_service_node')
        # 2. Create the active service server
        # Format: create_service(SrvType, 'service_name', callback_function)
        self.srv = self.create_service(
            SetSpeed,
            '/set_robot_speed',
            self.handle_set_speed
        )
        self.get_logger().info('Active Service /set_robot_speed is running!')

    # 3. Define the callback function (executes when someone calls the service)
    def handle_set_speed(self, request, response):
        # Access request fields
        self.get_logger().info(f'Received: speed={request.speed}, direction={request.direction}')

        # Populate response fields
        if request.speed >= 0.0:
            response.success = True
            response.message = f'Speed set to{request.speed}{request.direction}'
        else:
            response.success = False
            response.message = 'Error: Speed cannot be negative!'

        # Return the completed response back to the client
        return response

def main(args=None):
    rclpy.init(args=args)
    node = SpeedServiceNode()
    rclpy.spin(node) # Keeps the node running to listen for calls
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

- **How to Trigger Your Active Service**:

	- Run the Python node: `ros2 run my_custom_package speed_service`

	- **Verify it is active**:

		
```bash
ros2 service list
# Output will show: /set_robot_speed
```

	- **Call it directly from the terminal**:

		
```bash
ros2 service call /set_robot_speed my_custom_package/srv/SetSpeed "{speed: 1.5, direction: 'forward'}"
```

- **ROS2 Parameters (Node Configs)**

	- Configuration values stored inside nodes (e.g., maximum velocity limits, serial port names).

	- Declaring parameters in Python:

		
```python
self.declare_parameter('max_speed', 0.5)
```

	- Accessing parameters at runtime:

		
```python
current_limit = self.get_parameter('max_speed').value
```

	- Command-line interface parameters:

- `ros2 param list`: Lists parameters of all nodes.

- `ros2 param get /my_node max_speed`: View value.

- `ros2 param set /my_node max_speed 1.2`: Change value on the fly.

- **Loading Parameters from YAML files**

	- Store system parameters in a single configuration file:

		
```yaml
# config.yaml
/my_node:
ros__parameters:
max_speed:1.5
port:"/dev/ttyUSB0"
```

	- Launch nodes with the parameters file:

		
```bash
ros2 run my_pkg my_node_exe --ros-args --params-file config.yaml
```

---

### 🛠️ Hands-on Lab

- Implement a Python service server node that changes a motor state parameter (e.g., “STOPPED”, “RUNNING”) when receiving a request.

- Test calling this service server using CLI commands.

---

### 📝 Assignment

- **Task**: Create a node `safety_monitor` with parameters for safety range limits and battery threshold values. Add a service interface `/reconfigure` allowing users to change these parameters via terminal calls. Dump the updated parameter state to a local file using `ros2 param dump`.

