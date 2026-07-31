# Day 03: Python OOP & Concurrency for Robots

### 📌 TL;DR
Review Python scripting, isolate packages using virtual environments, write OOP classes (essential for writing ROS2 nodes), and use multithreading to handle parallel sensor updates.

---

### 📖 The Core Topics

* **Virtual Environments (venv)**
  * Isolate libraries to prevent breaking system-wide dependencies.
  * Create environment: `python3 -m venv rob_env`.
  * Activate: `source rob_env/bin/activate`.
  * Manage dependencies: `pip install -r requirements.txt`.
* **OOP (Object-Oriented Programming) for Robots**
  * ROS2 nodes are written as Python classes inheriting from the base Node class.
  * Essential Python class components:
    * `self`: Refers to the specific object instance.
    * `__init__()`: Constructor method called when initializing an object.
    * Inheritance: Subclass inherits attributes/methods from a parent class.
  * Code snippet representing standard inheritance layout:
    ```python
    class Robot:
        def __init__(self, name):
            self.name = name
            
    class SurgicalRobot(Robot):
        def __init__(self, name, joints_count):
            super().__init__(name)
            self.joints = joints_count
    ```
* **Multithreading & Concurrency**
  * Why multithreading? A robot must read sensors (IMU, LiDAR) in the background while processing navigation math on the main thread.
  * Use the `threading` library to run parallel loops.
  * Race Conditions & Lock: Protect shared resources (like motor target inputs) using locks so threads don't overwrite each other.
    ```python
    from threading import Lock
    lock = Lock()
    with lock:
        # thread-safe modifications
        robot_position += 1
    ```
* **Handling Errors & Exceptions**
  * Don't let your robot crash! Wrap hardware communications in try-except blocks:
    ```python
    try:
        read_serial_data()
    except Exception as e:
        print(f"Error reading sensor: {e}")
    ```

---

### 🛠️ Hands-on Lab
* Create a virtual environment `robot_env`.
* Write a script running two threads:
  1. Thread 1: Simulates motor position updates at 50Hz.
  2. Thread 2: Simulates IMU orientation updates at 100Hz.
  * Log states thread-safely using a global variable and a lock.

---

### 📝 Assignment
* **Task**: Create a Python CLI simulator (`console_robot.py`) featuring a `Robot` parent class, a `SurgicalRobot` child class, dynamic state updates, thread-safe logging, and standard try-except error catching.
