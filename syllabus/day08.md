---
notion-url: https://www.notion.so/3aec4db880d9818e92e0d0bbd4f8246e
title: day08
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/day08-3aec4db880d9818e92e0d0bbd4f8246e
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Day 08: ROS2 Actions (Asynchronous Goals & Feedback)

### 📌 TL;DR

---

### 📖 The Core Topics

- **Why Actions? (Topics vs. Services vs. Actions)**

	- **Topics**: Unidirectional data streams (e.g., IMU readings at 100Hz).

	- **Services**: Quick request-reply blocks (e.g., toggle camera state).

	- **Actions**: Long-term tasks that can take seconds or minutes. They allow:

- **Feedback**: Updates on task progress (e.g., “Navigated 40% of the way”).

- **Cancellation**: Stop the task mid-execution (e.g., “Stop navigation, obstacle ahead!”).

- **The Action Definition file (.action)**

	- Contains three sections separated by `--`:

		
```plain text
# NavigateToPose.action
# Section 1: Goal (Sent by client)
geometry_msgs/Pose target_pose
---
# Section 2: Result (Sent by server on finish)
bool arrived
float64 time_taken
---
# Section 3: Feedback (Sent by server continuously)
float64 distance_remaining
float64 current_velocity
```

- **Action Server Architecture**

	- Requires importing the action client/server package:

		
```python
from rclpy.action import ActionServer
```

	- Coordinates incoming goals, spins processing threads, publishes feedback loops, and sends the final execution result to the client.

- **CLI Action Commands**

	- `ros2 action list`: List active action servers.

	- `ros2 action send_goal /navigate_to_pose my_interfaces/action/NavigateToPose "{target_pose: {...}}"`: Trigger action from shell. Add `-feedback` to stream progress outputs.

---

### 🛠️ Hands-on Lab

- Create a custom action interface `ComputeTrajectory.action`.

- Implement a Python action server node `trajectory_sim_server` that accepts joint configurations, calculates simulated paths, publishes percentage-complete updates, and returns a confirmation result.

---

### 📝 Assignment

- **Task**: Develop a Python action client `trajectory_client` that connects to your `trajectory_sim_server` node. The client must submit target joints, print incoming feedback variables, handle task cancellation if the calculation runtime exceeds 5 seconds, and print the final result payload.

