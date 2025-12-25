# Plex Media Server in Docker

This is a concise guide to install Plex Media Server in a Docker container on a Raspberry Pi.

## 1. Prerequisites

* Raspberry Pi with Docker installed
* Docker Compose installed (optional but recommended)
* Internet connection

## 2. Create Plex Docker Directory

```bash
mkdir -p ~/docker/plex
cd ~/docker/plex
```

## 3. Create Docker Compose File

Create `docker-compose.yml` with the following content:

[../plex/docker-compose.yml](../plex/docker-compose.yml)

## 4. Start Plex

```bash
docker-compose up -d
```

## 5. Access Plex

Open a browser and go to:

```
http://<RPI_IP>:32400/web
```

Follow the setup wizard to sign in and configure libraries.

## 6. Notes

* Plex config is stored in `./config`. You can `.gitignore` this folder to avoid pushing runtime data to GitHub.
* Make sure your media directory is accessible to the Plex container.
