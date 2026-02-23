# Home Automation Stack (Docker Compose + Zigbee)

This guide sets up a fully reproducible, Git-backed home automation
stack using:

-   Home Assistant
-   Mosquitto (MQTT broker)
-   Zigbee2MQTT
-   SMLIGHT Zigbee coordinator
-   Docker Compose

The goal:\
If your Raspberry Pi dies, you can restore everything with:

    git clone
    docker compose up -d

------------------------------------------------------------------------

# 1. Prerequisites

## Hardware

-   Raspberry Pi
-   SMLIGHT Zigbee coordinator (SLAB-06MU)
-   Network cable

## Software

-   Raspberry Pi OS Lite (64-bit recommended)
-   Docker
-   Docker Compose (plugin version recommended)

Install Docker:

    curl -fsSL https://get.docker.com | sh
    sudo usermod -aG docker $USER

Reboot after installation.

------------------------------------------------------------------------

# 2. Project Structure

Create a clean folder structure:

    home-automation/
    ├── docker-compose.yml
    ├── .env
    ├── homeassistant/
    │   └── configuration.yaml
    ├── mosquitto/
    │   ├── config/
    │   │   └── mosquitto.conf
    │   ├── data/
    │   └── log/
    └── zigbee2mqtt/
        └── data/
            └── configuration.yaml

Everything except runtime data can be committed to Git.

------------------------------------------------------------------------

# 3. docker-compose.yml (Detailed + Commented)

Create:

    home-automation/docker-compose.yml

------------------------------------------------------------------------

```yml

services:

  # ------------------------------------------------------------
  # MQTT Broker
  # ------------------------------------------------------------
  mosquitto:
    image: eclipse-mosquitto:latest
    container_name: mosquitto
    restart: unless-stopped

    # Mount configuration and persistent storage
    volumes:
      - ./mosquitto/config:/mosquitto/config
      - ./mosquitto/data:/mosquitto/data
      - ./mosquitto/log:/mosquitto/log

    # Expose MQTT port
    ports:
      - "1883:1883"

  # ------------------------------------------------------------
  # Zigbee2MQTT (Ethernet/TCP)
  # ------------------------------------------------------------
  zigbee2mqtt:
    image: koenkk/zigbee2mqtt:latest
    container_name: zigbee2mqtt
    restart: unless-stopped

    # Wait for MQTT broker to start first
    depends_on:
      - mosquitto

    # Persistent data
    volumes:
      - ./zigbee2mqtt/data:/app/data

    # Web frontend
    ports:
      - "8080:8080"

    # Environment variables
    environment:
      - TZ=Europe/London

    # TCP connection — no USB devices needed
    # The serial config is defined in zigbee2mqtt/data/configuration.yaml:
    # serial:
    #   port: tcp://192.168.1.72:6638
    #   adapter: ezsp

  # ------------------------------------------------------------
  # Home Assistant
  # ------------------------------------------------------------
  homeassistant:
    image: ghcr.io/home-assistant/home-assistant:stable
    container_name: homeassistant
    restart: unless-stopped

    # Host networking is required for discovery integrations
    network_mode: host

    # Config folder
    volumes:
      - ./homeassistant:/config

    # Environment variables
    environment:
      - TZ=Europe/London

```
------------------------------------------------------------------------

------------------------------------------------------------------------


# 4. Mosquitto Configuration

Create:

    mosquitto/config/mosquitto.conf


```conf

persistence true
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log

listener 1883
allow_anonymous true

```

------------------------------------------------------------------------

(You can secure MQTT with passwords later.)

------------------------------------------------------------------------

# 5. Zigbee2MQTT Configuration

Create:

    zigbee2mqtt/data/configuration.yaml

find ip address from router and add to config
```yml

# Enable Home Assistant integration
homeassistant: true

# MQTT broker configuration
mqtt:
  base_topic: zigbee2mqtt
  server: mqtt://mosquitto:1883

# Zigbee coordinator connection (Ethernet/TCP)
serial:
  port: tcp://192.168.1.72:6638  # <- Replace with your SLZB-06MU IP
  adapter: ezsp

# Web frontend configuration
frontend:
  port: 8080

# Allow pairing of new devices
permit_join: false

# Advanced network settings
advanced:
  network_key: GENERATE

```

Important: Back up the generated network key.\
If lost, you must re-pair all devices.

------------------------------------------------------------------------

# 6. First Deployment

From inside the project folder:

    docker compose up -d

Check logs:

    docker compose logs -f

------------------------------------------------------------------------

# 7. Access Interfaces

Home Assistant: http://`<pi-ip>`{=html}:8123

Zigbee2MQTT: http://`<pi-ip>`{=html}:8080

------------------------------------------------------------------------

# 8. Pairing Devices

1.  Open Zigbee2MQTT UI
2.  Click "Permit Join"
3.  Put device in pairing mode
4.  Device appears automatically in Home Assistant

------------------------------------------------------------------------

# 9. Git Best Practices

Add .gitignore:

    mosquitto/data/
    mosquitto/log/
    zigbee2mqtt/data/database.db

Commit everything else.


------------------------------------------------------------------------

# 10. Recommended Next Steps

-   Add MQTT authentication
-   Add automatic backups
-   Add reverse proxy (Traefik or Nginx)
-   Add remote access via VPN
-   Place Zigbee coordinator on extension cable
-   Use powered Zigbee devices to strengthen mesh

------------------------------------------------------------------------

You now have a clean, version-controlled, reproducible home automation
stack.
