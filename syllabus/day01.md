---
notion-url: https://www.notion.so/3aec4db880d98175abc1d5dba44e3c7a
---

# Day 01: Ubuntu & Linux Fundamentals

### 📌 TL;DR
Get Ubuntu installed (dual-boot or VM), learn the basic file structure, and master standard bash terminal commands to move, edit, and manage files without a GUI.

---

### 📖 The Core Topics

* **Ubuntu Installation & Setup**
  * Use Ubuntu 22.04 LTS (Jammy Jellyfish) to match ROS2 Humble.
  * Dual-boot: Install alongside Windows (best performance for graphics/simulations).
  * Virtual Machine (VM): Use VirtualBox or VMware if you don't want to partition your hard drive. Allocating resources:
    * Minimum: 4 CPU cores, 8GB RAM, 50GB space.
* **Linux File Hierarchy (What is where?)**
  * `/` (root): The parent of all folders.
  * `/bin` & `/sbin`: Core system commands and binaries.
  * `/etc`: System configuration files.
  * `/home`: Your workspace (`~/`). Where user files live.
  * `/opt`: Third-party software (this is where ROS2 installs!).
  * `/dev`: Connected hardware files (USBs, serial ports, cameras).
* **Must-Know Terminal Commands**
  * `pwd`: Print working directory.
  * `ls -la`: List files, showing hidden ones (`-a`) and detailed info (`-l`).
  * `cd <dir>`: Change directory.
  * `mkdir -p <path>`: Create nested directories.
  * `touch <file>`: Create an empty file.
  * `cp -r <src> <dest>`: Copy directories recursively.
  * `mv <src> <dest>`: Move or rename files/folders.
  * `rm -rf <path>`: Force delete files/folders recursively.
  * `grep -rn "search_term" <dir>`: Search for text within files recursively.
* **Permissions & Administrative Power**
  * Three permission levels: Read (`r` = 4), Write (`w` = 2), Execute (`x` = 1).
  * `chmod +x script.sh`: Make a script executable.
  * `chmod 755 file`: Owner can do all; group/others can only read/execute.
  * `chown user:group file`: Change owner/group of a file.
  * `sudo`: Run commands with admin (root) privileges.
* **Editing Files in the Terminal**
  * `nano`: Beginner-friendly text editor.
  * `vim`: Modal editor (Press `i` to type, `Esc` then `:wq` to save and exit).
* **SSH (Secure Shell)**
  * Accessing another terminal over the network: `ssh user@ip_address`.
  * Set up ssh server: `sudo apt install openssh-server`.
* **APT Package Manager**
  * `sudo apt update`: Pull latest list of available packages.
  * `sudo apt upgrade`: Update installed packages.
  * `sudo apt install <package>`: Install software.

---

### 🛠️ Hands-on Lab
* Set up Ubuntu (LTS 22.04).
* Install VS Code and Git:
  ```bash
  sudo apt update
  sudo apt install code git -y
  ```
* Configure your Git credentials:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  ```

---

### 📝 Assignment
* **Task**: Create a terminal cheat sheet named `linux_cheat_sheet.md` with your top 30 command lines and descriptions.
