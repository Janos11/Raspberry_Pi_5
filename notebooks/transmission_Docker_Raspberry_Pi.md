# Transmission in Docker (Raspberry Pi)

This document describes a **simple, permission-safe setup** for running Transmission inside Docker on a Raspberry Pi.

---

## 1. Prerequisites

- Raspberry Pi (ARM64/ARMv7)
- Docker and Docker Compose installed
- User `pi`
- A downloads directory on disk (example used below)

Example downloads directory:
```
/media/pi/data/downloads
```

---

## 2. Why Docker for Transmission?

- Avoids Linux permission issues
- No system services or daemon conflicts
- Easy restart on boot
- Clean removal if something breaks

---

## 3. Docker Compose File

Create a file called:

```
docker-compose.yml
```

Link to actual file: [docker-compose.yml](../transmission/docker-compose.yml)

---

## 4. Directory Preparation (IMPORTANT)

On the host (Raspberry Pi):

```bash
mkdir -p config watch
sudo chown -R pi:pi config watch /media/pi/data/downloads
```

This ensures Transmission can **read and write files correctly**.

---

## 5. Start Transmission

From the directory containing `docker-compose.yml`:

```bash
docker compose up -d
```

Check status:
```bash
docker ps
```

---

## 6. Access Web UI

Open in browser:

```
http://<raspberry-pi-ip>:9091
```

Example:
```
http://192.168.1.50:9091
```

---

## 7. Watch Directory (Optional)

**What it does:**
- Any `.torrent` file placed into `./watch` is automatically added to Transmission.

**Does it break torrents?**
- ❌ No
- It only adds torrents, it does not touch existing data.

---

## 8. Reusing Existing Downloads (Important)

If Transmission starts re-downloading files:

1. Stop the torrent
2. Right-click → **Set Location**
3. Point to the folder where files already exist
4. Right-click → **Verify Local Data**
5. Start torrent → it should **seed instead of download**

---

## 9. Permissions Troubleshooting

Check ownership on host:
```bash
ls -ld /media/pi/data/downloads
```

Expected:
```
pi pi
```

If not:
```bash
sudo chown -R pi:pi /media/pi/data/downloads
```

---

## 10. Auto Start on Boot

Already handled by Docker:

```yaml
restart: unless-stopped
```

No systemd or GUI autostart required.

---

## Done 🎉

You now have:
- Stable Transmission
- No permission headaches
- Clean Docker-based setup
