---
notion-url: https://www.notion.so/3aec4db880d981809cb3e5a24d37bf31
---

# Day 10: TF2, Robot Frames & The Math of Kinematics

### 📌 TL;DR
Robots are made of links and joints. To know where the hand is relative to the base, we use math: Rotation Matrices, Translation Vectors, Quaternions, and Kinematics. TF2 in ROS2 manages all of this automatically in the background.

---

### 📖 Math Concepts: What is What?

#### 1. Coordinate Frames
* A coordinate frame is a 3D axis ($X, Y, Z$) attached to parts of a robot (e.g., `base_link`, `camera_link`, `wrist_link`).
* Position is easy: a vector $[x, y, z]^T$ representing distance.
* Orientation is hard: how is the frame tilted relative to another? We use Rotation Matrices or Quaternions.

#### 2. Rotation Matrices
* A 3D rotation is represented by a $3 \times 3$ matrix ($R$).
* Columns of $R$ are unit vectors pointing along the rotated axes relative to the original frame:
  $$R = \begin{bmatrix} r_{11} & r_{12} & r_{13} \\ r_{21} & r_{22} & r_{23} \\ r_{31} & r_{32} & r_{33} \end{bmatrix}$$
* Properties:
  * Orthonormal: Transpose is its inverse ($R^T = R^{-1}$).
  * Determinant is 1: $\det(R) = 1$ (no scaling or mirroring).

#### 3. Homogeneous Transformation Matrices
* To combine rotation ($R$) and translation (vector $T = [x, y, z]^T$) into one calculation, we use a $4 \times 4$ Matrix ($T_{H}$):
  $$T_H = \begin{bmatrix} & R & & T \\ 0 & 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} r_{11} & r_{12} & r_{13} & x \\ r_{21} & r_{22} & r_{23} & y \\ r_{31} & r_{32} & r_{33} & z \\ 0 & 0 & 0 & 1 \end{bmatrix}$$
* Why is this useful?
  * To chain links together: $T_{base \to hand} = T_{base \to elbow} \times T_{elbow \to wrist} \times T_{wrist \to hand}$.
  * Simply multiply the matrices to get the final location!

#### 4. Euler Angles vs. Quaternions
* **Euler Angles** (Roll, Pitch, Yaw):
  * Easy to visualize (e.g., Yaw = spin left/right).
  * Drawback: **Gimbal Lock** (losing a degree of freedom when two axes align at 90 degrees).
* **Quaternions**:
  * Represented as 4 numbers: $q = [w, x, y, z]$.
  * No Gimbal Lock, easier for computers to compute, but impossible for humans to read.
  * ROS2 uses **Quaternions** for all rotations in messages.

#### 5. Forward Kinematics (FK)
* **What is it?** Inputting joint angles ($\theta_1, \theta_2, \dots$) and computing where the robot's tip (end-effector) is in 3D space ($x, y, z$, roll, pitch, yaw).
* Uses simple geometry and matrix multiplication.

#### 6. Inverse Kinematics (IK)
* **What is it?** Inputting the desired 3D coordinates ($x, y, z$) of the hand, and computing the joint angles ($\theta_1, \theta_2, \dots$) needed to get there.
* Way harder than FK because:
  * Multiple solutions exist (you can reach a point with elbow-up or elbow-down).
  * No solution might exist (point is out of reach).

---

### 📡 TF2 in ROS2 (Transform Library)

* **How TF2 works**:
  * TF2 builds a **Tree** of coordinate frames. No loop is allowed (each frame has exactly one parent).
  * Nodes broadcast relationships (e.g., `base_link` $\to$ `camera_link`).
  * Other nodes lookup transforms (e.g., "Give me the vector from `camera_link` to `gripper_link` right now").
* **Static Transform Broadcaster**:
  * Used for parts that do not move relative to each other (e.g., camera mounted on base).
  * Command line quick publisher:
    ```bash
    ros2 run tf2_ros static_transform_publisher --x 0.1 --y 0 --z 0.5 --yaw 0 --pitch 0 --roll 0 --frame-id base_link --child-frame-id camera_link
    ```
* **Dynamic Transform Broadcaster**:
  * Used for moving parts (e.g., joints rotation).
  * Python implementation:
    ```python
    from tf2_ros import TransformBroadcaster
    from geometry_msgs.msg import TransformStamped

    # Inside node:
    self.tf_broadcaster = TransformBroadcaster(self)
    t = TransformStamped()
    t.header.stamp = self.get_clock().now().to_msg()
    t.header.frame_id = 'base_link'
    t.child_frame_id = 'link_1'
    t.transform.translation.x = 0.2
    # Fill in rotation quaternion
    self.tf_broadcaster.sendTransform(t)
    ```
* **Looking up Transforms (TF Listener)**:
  * Uses a Buffer to store past transforms and queries them:
    ```python
    from tf2_ros import Buffer, TransformListener
    # Inside init:
    self.tf_buffer = Buffer()
    self.tf_listener = TransformListener(self.tf_buffer, self)
    # Inside timer:
    trans = self.tf_buffer.lookup_transform('base_link', 'camera_link', rclpy.time.Time())
    ```
* **TF2 CLI Tools**:
  * `ros2 run tf2_tools view_frames`: Generates a PDF diagram showing the active coordinate frame tree.
  * `ros2 run tf2_ros tf2_echo base_link link_1`: Monitor translation and rotation between two frames in real-time.

---

### 🛠️ Hands-on Lab
* Start a dynamic transform node:
  ```bash
  ros2 run turtlesim turtlesim_node
  ```
* Run turtle broadcaster that publishes turtle position as a coordinate frame:
  ```bash
  ros2 run turtle_tf2_py turtle_tf2_broadcaster
  ```
* Run `view_frames` to generate a PDF map of the active system transform tree.

---

### 📝 Assignment
* **Task**: Write a python node `robot_tf_broadcaster.py` that broadcasts coordinate transforms for a 4-DoF arm: `base_link` $\to$ `link_1` $\to$ `link_2` $\to$ `link_3` $\to$ `tool_tip`. Link_1 rotates continuously using `sin(time)` parameter.
