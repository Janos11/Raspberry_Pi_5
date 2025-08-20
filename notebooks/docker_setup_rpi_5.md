# Installing Docker on Raspberry Pi 5
20 August 2025

Follow these steps to install Docker and set up your user for Docker access.

---

## 1. Update and Upgrade Packages

```bash
sudo apt update
sudo apt upgrade -y
```

---

## 2. Install Docker

```bash
curl -sSL https://get.docker.com | sh
```

> This script installs the latest Docker engine on your Raspberry Pi.

---

## 3. Add Your User to the Docker Group

```bash
sudo usermod -aG docker $USER
```

> This allows running Docker commands without `sudo`.

---

## 4. Log Out and Back In

- Log out of your session and log back in (or reboot) to apply the group changes.

---

## 5. Verify Docker Installation

```bash
docker ps
docker ps -a
which docker
systemctl show docker
groups
```

- `docker ps` → shows running containers  
- `docker ps -a` → shows all containers  
- `which docker` → confirms Docker binary location  
- `systemctl show docker` → checks Docker service status  
- `groups` → ensures your user is in the `docker` group

---

## 6. Optional: Create a Working Directory

```bash
mkdir ~/docker
cd ~/docker
```

> Use this directory for Docker Compose files and container projects.

