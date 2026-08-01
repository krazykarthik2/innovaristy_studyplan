---
notion-url: https://www.notion.so/3aec4db880d9812d8200fa23c5aa2f23
title: day05
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/day05-3aec4db880d9812d8200fa23c5aa2f23
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Day 05: ROS2 Nodes, Topics, Messages & Publishers

### 📌 TL;DR

---

### 📖 The Core Topics

- **ROS2 Nodes (What are they?)**

	- Individual executables responsible for one specific task (e.g., node 1 reads camera, node 2 plans path, node 3 outputs motor command).

	- Run a node: `ros2 run <package_name> <executable_name>`.

	- Node commands:

- `ros2 node list`: List active nodes on network.

- `ros2 node info /my_node`: View subscriptions, publications, services, and actions for `/my_node`.

- **ROS2 Topics (How do they talk?)**

	- One-way data communication channels.

	- Publishers write data, Subscribers read data.

	- A topic can have multiple publishers and subscribers (many-to-many).

	- Anonymous: Nodes don’t need to know which other nodes are listening; they just publish to the topic.

	- Topic inspection commands:

- `ros2 topic list`: Shows all active topics.

- `ros2 topic echo /my_topic`: Prints the messages published on `/my_topic` in real-time.

- `ros2 topic info /my_topic`: Shows active publishers and subscribers count.

- `ros2 topic hz /my_topic`: Measure data publishing frequency.

	- **Visualizing Node Graphs (**`**rqt_graph**`**)**

- `rqt_graph`: A GUI tool that draws the active system graph. Essential for debugging node connections.

- **ROS2 Interfaces (.msg, .srv, .action)**

	- **What is an Interface?**: A structured contract that defines the format of data sent between nodes.

	- Three core types of interfaces:

1. **Messages (.msg)**: Unidirectional structures used by **Topics** (e.g., streaming IMU or battery telemetry).

1. **Services (.srv)**: Request/Response structures used by **Services** (e.g., triggering a camera capture).

1. **Actions (.action)**: Goal/Result/Feedback structures used by **Actions** (e.g., commanding a joint trajectory plan).

- Built-in interfaces: `std_msgs/msg/String`, `sensor_msgs/msg/LaserScan`, `geometry_msgs/msg/Pose`.

- CLI inspection commands:

- `ros2 interface list`: Show all messages, services, and actions installed on your system.

- `ros2 interface show geometry_msgs/msg/Point`: View layout details (e.g., `float64 x`, `float64 y`, `float64 z`).

- **Writing your first Publisher Node (Python)**

	- Uses `rclpy` (ROS2 Client Library for Python).

	- Structure of a node class:

		
```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class Talker(Node):
    def __init__(self):
        # Give the node a name
        super().__init__('talker_node')
        # Initialize publisher: Type, Topic Name, Queue Size (QoS)
        self.publisher_ = self.create_publisher(String, 'chatter', 10)
        # Set up timer to call publisher callback at 1 Hz
        self.timer = self.create_timer(1.0, self.timer_callback)

    def timer_callback(self):
        msg = String()
        msg.data = 'Hello, ROS2!'
        self.publisher_.publish(msg)
        self.get_logger().info(f'Published: "{msg.data}"')

def main(args=None):
    rclpy.init(args=args)
    node = Talker()
    rclpy.spin(node) # Keeps node running, checking for events
    node.destroy_node()
    rclpy.shutdown()
```

---

### 🛠️ Hands-on Lab

- Run demo nodes to test your setup:

	- In Terminal 1: `ros2 run demo_nodes_py talker`

	- In Terminal 2: `ros2 run demo_nodes_py listener`

- Use CLI tools:

	- Check node relationships: `ros2 node list`

	- Echo the telemetry stream: `ros2 topic echo /chatter`

	- Open the graphical connection map: `rqt_graph`

---

### 📝 Assignment

- **Task**: Create two nodes from scratch:

	1. `telemetry_publisher.py`: Publishes simulated battery level (`std_msgs/msg/Float32`) to `/battery_status` at 2Hz.

	1. `alarm_subscriber.py`: Subscribes to `/battery_status` and triggers a terminal warning alert if the charge falls below 20.0.

