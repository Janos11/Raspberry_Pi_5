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

## 7. Ignoring Sample and Unwanted Files (`.plexignore`)

Plex supports a `.plexignore` file to exclude specific folders or files from being scanned (similar to `.gitignore`).

Create a file named `.plexignore` **inside the directory where your movies are stored** (the same directory you add as a Plex library).

```bash
nano /path/to/your/media/.plexignore
```

Example contents (ignore sample folders such as Hungarian minta, and macOS junk files):

```text
# Ignore sample folders
minta/
*Minta*/
*mint*/

# Ignore macOS metadata files
.DS_Store
._*
```
Important
The .plexignore file must be placed in the media directory Plex scans
It applies to that directory and all subdirectories
After creating or modifying it, rescan the library in Plex
This prevents Plex from mistakenly selecting short sample clips instead of the main movie file.
