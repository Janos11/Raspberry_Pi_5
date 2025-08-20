# Useful Raspberry Pi & Linux Commands

This document lists commonly used **Raspberry Pi-specific** and **Linux system** commands.  
They are grouped by category and include short descriptions.

---

## 🖥️ System Monitoring

| Command             | Description                                      |
|---------------------|--------------------------------------------------|
| `top`               | Show running processes and CPU/memory usage.     |
| `htop`              | Interactive process viewer (requires install).   |
| `free -h`           | Show memory usage in human-readable format.      |
| `uptime`            | Show system uptime and load average.             |
| `vcgencmd measure_temp` | Show Raspberry Pi CPU/GPU temperature.        |
| `vcgencmd get_mem arm`  | Show memory assigned to ARM (CPU).            |
| `vcgencmd get_mem gpu`  | Show memory assigned to GPU.                   |

---


## 💾 Disk & Storage

| Command             | Description                                      |
|---------------------|--------------------------------------------------|
| `df -h`             | Show disk usage in human-readable format.        |
| `du -sh *`          | Show size of each item in current directory.     |
| `lsblk`             | List block devices (drives, partitions).         |
| `mount`             | Show mounted filesystems.                        |

---

## 🔌 Services & Processes

| Command                     | Description                                |
|------------------------------|--------------------------------------------|
| `systemctl status ssh`       | Check status of SSH service.               |
| `sudo systemctl enable ssh`  | Enable SSH to start on boot.               |
| `sudo systemctl start ssh`   | Start SSH service immediately.             |
| `sudo systemctl stop ssh`    | Stop SSH service.                          |
| `ps aux`                     | Show all running processes.                |
| `kill -9 <PID>`              | Kill a process by PID.                     |

---

## 🌐 Networking

| Command                 | Description                                   |
|--------------------------|-----------------------------------------------|
| `hostname -I`            | Show device’s IP addresses.                  |
| `ping <host>`            | Test network connectivity.                   |
| `ifconfig`               | Show network interfaces (legacy).            |
| `ip a`                   | Show network interfaces (modern).            |
| `netstat -tulnp`         | Show open ports and listening services.      |
| `ss -tulwn`              | Alternative to netstat for sockets.          |
| `traceroute <host>`      | Show route packets take to a host.           |

---

## 🔧 System Management

| Command                     | Description                                |
|------------------------------|--------------------------------------------|
| `passwd`                     | Change current user’s password.            |
| `sudo raspi-config`          | Raspberry Pi configuration tool.           |
| `sudo reboot`                | Reboot system.                             |
| `sudo shutdown -h now`       | Shutdown immediately.                      |
| `sudo shutdown -r now`       | Reboot immediately.                        |
| `uname -a`                   | Show kernel and OS details.                |
| `lsusb`                      | List USB devices.                          |
| `lscpu`                      | Show CPU details.                          |

---

## 📂 File & Directory Management

| Command                 | Description                                   |
|--------------------------|-----------------------------------------------|
| `ls -lh`                 | List files with sizes in human-readable form.|
| `cd <dir>`               | Change directory.                            |
| `pwd`                    | Show current working directory.              |
| `mkdir <dir>`            | Create a new directory.                      |
| `rm <file>`              | Delete a file.                               |
| `rm -r <dir>`            | Delete a directory and contents recursively. |
| `cp <src> <dest>`        | Copy a file.                                 |
| `mv <src> <dest>`        | Move or rename a file.                       |
| `nano <file>`            | Open a text editor in terminal.              |
| `cat <file>`             | Print contents of a file.                    |
| `less <file>`            | View file contents page by page.             |

---

## 🔒 User & Permissions

| Command                   | Description                                 |
|----------------------------|---------------------------------------------|
| `whoami`                  | Show current logged-in user.                |
| `id`                      | Show user ID and group information.         |
| `groups`                  | Show groups current user belongs to.        |
| `sudo adduser <name>`      | Create a new user.                          |
| `sudo deluser <name>`      | Delete a user.                              |
| `chmod 755 <file>`        | Change file permissions.                    |
| `chown user:group <file>` | Change file ownership.                      |

---

## 🔍 Useful Information

| Command                | Description                                    |
|-------------------------|------------------------------------------------|
| `dmesg | less`          | Show kernel/system logs.                       |
| `journalctl -xe`        | Show detailed system logs (systemd).           |
| `history`               | Show command history.                          |
| `which <command>`       | Show location of a command.                    |
| `man <command>`         | Show manual/help for a command.                |

---

✅ These commands cover **system monitoring, networking, storage, processes, and Raspberry Pi-specific tools**.  
They allow full control and troubleshooting of a Raspberry Pi or Linux machine from the command line.
