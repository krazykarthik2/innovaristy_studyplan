---
notion-url: https://www.notion.so/3aec4db880d98158ba6dcfde55216d10
---

# Day 02: Linux for Robotics & Automation

### 📌 TL;DR
Learn process monitoring, shell scripting for automating builds/setups, network utilities, and how to assign a static IP for multi-robot setups.

---

### 📖 The Core Topics

* **Process Management & Signals**
  * Check running processes using `htop` (visual monitor) or `ps aux`.
  * Background running: Add `&` at the end of a command (e.g., `ros2 run pkg node &`).
  * Process control commands:
    * `jobs`: List background jobs.
    * `fg %1`: Bring job 1 to foreground.
    * `bg %1`: Resume job 1 in background.
  * Killing processes:
    * `kill -9 <PID>`: Instantly kill process by ID (SIGKILL).
    * `killall <process_name>`: Kill all processes with matching name (e.g., `killall gzserver`).
* **Environment Variables**
  * Shell variables that configure behaviors.
  * System-wide variables are stored in `~/.bashrc`.
  * `export ROS_DOMAIN_ID=30`: Restricts ROS2 traffic to domain ID 30.
  * View variables: `echo $PATH` or `printenv`.
* **Bash Scripting Basics**
  * Files starting with `#!/bin/bash` (the shebang).
  * Always use `set -e` at the start of your script to stop execution if any command fails.
  * Declaring variables: `MY_DIR="/home/user"` (No spaces around `=`).
  * Arguments: `$1` represents first input argument, `$2` second, etc.
* **Linux Networking for Robotics**
  * Inter-robot communication needs proper network tools:
    * `ip a` / `ifconfig`: View network interfaces and local IP.
    * `ping <ip>`: Check if target computer is reachable over network.
    * `nmap <ip>`: Find open ports on the target computer.
* **Static IP Configurations**
  * DHCP changes your robot's IP on reboot, which breaks connection strings.
  * Set a static IP via Netplan configuration file:
    ```yaml
    # /etc/netplan/01-netcfg.yaml
    network:
      version: 2
      renderer: networkd
      ethernets:
        eth0:
          dhcp4: no
          addresses: [192.168.1.100/24]
          gateway4: 192.168.1.1
    ```
  * Apply configuration: `sudo netplan apply`.

---

### 🛠️ Hands-on Lab
* Set up a static IP configuration for your workspace.
* Write a quick bash script (`diagnose.sh`) that checks if another computer is online (ping) and logs disk usage.

---

### 📝 Assignment
* **Task**: Write `ubuntu_setup.sh` to automate updating packages, installing git, python3-pip, htop, tmux, and appending your environment configurations to `~/.bashrc`.
