---
notion-url: https://www.notion.so/3aec4db880d98135b062e3115a4e3b2c
title: day15
date: '2026-07-31 20:18:00.000'
from_notion: https://app.notion.com/p/day15-3aec4db880d98135b062e3115a4e3b2c
author: Karthik KRAZY
last_edited_time: '2026-07-31 20:19:00.000'
---
# Day 15: Final Research Project Implementation (Practical)

### 📌 TL;DR

---

### 🛠️ Practical Project Execution Roadmap

[//]: # (heading_4 is not supported)

- Ensure your workspace `final_research_ws/` contains these exact packages:

	- `taurean_description`: Contains the URDF representation and mesh models.

	- `taurean_control`: Includes the publisher/subscriber nodes, safety managers, and custom actions.

	- `taurean_bringup`: Contains launch scripts and parameters configurations YAMLs.

[//]: # (heading_4 is not supported)

- Verify all dependencies are clean:

	
```bash
cd ~/final_research_ws
rosdep install --from-paths src --ignore-src -r -y
```

- Perform compilation:

	
```bash
colcon build --symlink-install
```

- Verify that compilation finishes with zero compilation errors.

[//]: # (heading_4 is not supported)

- Start your simulated robot launch script:

	
```bash
ros2 launch taurean_bringup robot_sim.launch.py
```

- Verify spatial coordinates are publishing to the tf branch:

	
```bash
ros2 run tf2_ros tf2_echo base_link end_effector
```

- Check for tree discontinuities by generating the coordinate tree frames PDF:

	
```bash
ros2 run tf2_tools view_frames
```

[//]: # (heading_4 is not supported)

- Trigger your safety node simulation:

	- Command a path execution node to move the end-effector outside the allowed Cartesian bounding box limits.

	- Verify the safety supervisor catches the violation, terminates the action goal, and logs error outputs.

---

### 📋 Final Submission Checklist

- [ ] **Code Repository**: Hosted on GitHub, clean directory structure, all compilation configurations tested.

- [ ] **System URDF**: Clean file with proper visual/collision boundaries and kinematics configurations.

- [ ] **Transforms (TF2)**: Active trees linking all frames from base to tool tips.

- [ ] **Documentation**: A `README.md` at root describing how to install, build, launch, and configure variables.

- [ ] **Demonstration Video**: Recording showing RViz movements, parameter server updates, and safety checks.

