---
notion-url: https://www.notion.so/3aec4db880d9813baa05c592fb53441a
---

# Day 11: URDF Robot Description & Kinematic Chains

### 📌 TL;DR
URDF is an XML file format that defines your robot's physical properties. You will describe links (visuals, collision bounding boxes, and weights) and joints (rotations, limits, and parent-child links).

---

### 📖 The Core Topics

* **What is a URDF?**
  * Unified Robot Description Format.
  * Tells ROS2 where robot parts are, how they are connected, and how they behave physically.
* **The `<link>` Tag: The Bones**
  * Describes a single rigid part of the robot.
  * Three main sub-tags:
    1. **`<visual>`**: What it looks like. Uses basic shapes (box, cylinder, sphere) or detailed mesh models (STL, Collada/DAE files).
    2. **`<collision>`**: Outer boundary for physics engines to check for contacts. Keep this simple (e.g., box instead of high-poly mesh) to avoid lag in physics computation.
    3. **`<inertial>`**: Physics properties. Contains:
       * `<mass value="X"/>` (Weight in kilograms).
       * `<inertia ixx="..." ixy="..." .../>` (Inertia tensor matrix - how hard it is to rotate on each axis).
* **The `<joint>` Tag: The Pivot Points**
  * Connects two links together (parent link $\to$ child link).
  * Common joint types:
    * **revolute**: Rotates around an axis, with hard limits (e.g., knee joint: 0 to 120 degrees).
    * **continuous**: Rotates indefinitely (e.g., wheels).
    * **prismatic**: Slides along an axis (e.g., elevator/piston).
    * **fixed**: Welded together (e.g., mounting brackets).
  * Child elements of a joint:
    * `<parent link="link_a"/>`
    * `<child link="link_b"/>`
    * `<origin xyz="0 0 0.5" rpy="0 0 0"/>` (Joint position relative to parent).
    * `<limit lower="-1.57" upper="1.57" effort="10" velocity="1.0"/>` (Limits in radians, max torque, and max speed).
* **Verifying URDF files**
  * Check for XML structure issues using command tool:
    ```bash
    check_urdf my_robot.urdf
    ```
  * Convert your URDF to a visual graph layout:
    ```bash
    urdf_to_graphiz my_robot.urdf
    ```
    This outputs a `.gv` structure showing parent-child links.

---

### 🛠️ Hands-on Lab
* Create a simple 2-link URDF XML file `planar_arm.urdf`.
* Add one base link box, a revolute joint, and a cylinder arm link.
* Parse the file through `check_urdf` to confirm configuration formatting validity.

---

### 📝 Assignment
* **Task**: Create the URDF model for a 4-DoF Robot Arm (`4dof_robot.urdf`). Define specific links, joint rotation axes, angular limits, masses, and visual dimensions.
