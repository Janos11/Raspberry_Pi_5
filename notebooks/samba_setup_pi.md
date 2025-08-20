# Samba Docker Setup on Raspberry Pi
20 August 2025

This document explains the process of setting up a Samba server in Docker on a Raspberry Pi, with writable network shares for the user `pi` and the `sharedmedia` group.

---

## 1. Create Shared Group on Host

We create a dedicated group to manage access to shared files:

```bash
sudo groupadd sharedmedia
sudo usermod -aG sharedmedia pi
```

- `groupadd sharedmedia` → creates the group  
- `usermod -aG sharedmedia pi` → adds user `pi` to the group  

Set proper ownership and permissions on host directories:

```bash
sudo chown -R pi:sharedmedia /home/pi
sudo chmod g+s /home/pi
sudo chown -R pi:sharedmedia /media/pi
sudo chmod g+s /media/pi
```

- `chown` → sets user and group ownership  
- `chmod g+s` → ensures new files inherit the group `sharedmedia`  

> **Note:** For external drives (NTFS/exFAT), ownership cannot be changed. Mount with `uid=1000,gid=1001,umask=002`.

---

## 2. Docker Compose Setup

Create a `docker-compose.yml` file:

```yaml
services:
  samba:
    image: dperson/samba
    container_name: samba
    restart: unless-stopped
    ports:
      - "137:137/udp"
      - "138:138/udp"
      - "139:139/tcp"
      - "445:445/tcp"
    environment:
      - USERID=1000          # pi's UID
      - GROUPID=1001         # sharedmedia GID
    volumes:
      - /media/pi:/mnt/pi5_media
      - /home/pi/:/mnt/Pi5_home
    command: >
      -u "pi;raspberry"
      -s "pi5_media;/mnt/pi5_media;yes;yes;yes;pi;rw;force user = pi, force group = sharedmedia"
      -s "Pi5_home;/mnt/Pi5_home;yes;yes;yes;pi;rw;force user = pi, force group = sharedmedia"
    network_mode: bridge
```

- `-s` defines Samba shares:  
  - `yes` flags enable browsing, guest access, and read/write  
  - `force user` and `force group` ensure files are created as `pi:sharedmedia`  

---

## 3. Start the Samba Container

```bash
docker compose up -d
```

- The shares `/mnt/pi5_media` and `/mnt/Pi5_home` are now accessible on the network  
- User `pi` has full read/write access due to forced user/group settings

---

## 4. Summary

- Created `sharedmedia` group and added `pi` to it  
- Adjusted ownership and permissions on host directories  
- Configured Docker Samba container with forced user/group for write access  
- Shares are now writable from the network with consistent permissions

