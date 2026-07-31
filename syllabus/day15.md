# Day 15: Final Research Project Implementation (Practical)

### 📌 TL;DR
The final practical challenge. You will assemble your packages, configure launches, test the kinematic transformation chains, run simulations in RViz, and deliver the final research repository.

---

### 🛠️ Practical Project Execution Roadmap

#### Step 1: Workspace Assembly
* Ensure your workspace `final_research_ws/` contains these exact packages:
  * `taurean_description`: Contains the URDF representation and mesh models.
  * `taurean_control`: Includes the publisher/subscriber nodes, safety managers, and custom actions.
  * `taurean_bringup`: Contains launch scripts and parameters configurations YAMLs.

#### Step 2: Sourcing and Compilation Check
* Verify all dependencies are clean:
  ```bash
  cd ~/final_research_ws
  rosdep install --from-paths src --ignore-src -r -y
  ```
* Perform compilation:
  ```bash
  colcon build --symlink-install
  ```
* Verify that compilation finishes with zero compilation errors.

#### Step 3: Run the Transform Verification
* Start your simulated robot launch script:
  ```bash
  ros2 launch taurean_bringup robot_sim.launch.py
  ```
* Verify spatial coordinates are publishing to the tf branch:
  ```bash
  ros2 run tf2_ros tf2_echo base_link end_effector
  ```
* Check for tree discontinuities by generating the coordinate tree frames PDF:
  ```bash
  ros2 run tf2_tools view_frames
  ```

#### Step 4: Validate Safety Boundaries
* Trigger your safety node simulation:
  * Command a path execution node to move the end-effector outside the allowed Cartesian bounding box limits.
  * Verify the safety supervisor catches the violation, terminates the action goal, and logs error outputs.

---

### 📋 Final Submission Checklist

* [ ] **Code Repository**: Hosted on GitHub, clean directory structure, all compilation configurations tested.
* [ ] **System URDF**: Clean file with proper visual/collision boundaries and kinematics configurations.
* [ ] **Transforms (TF2)**: Active trees linking all frames from base to tool tips.
* [ ] **Documentation**: A `README.md` at root describing how to install, build, launch, and configure variables.
* [ ] **Demonstration Video**: Recording showing RViz movements, parameter server updates, and safety checks.
