# Pi-hole Docker Setup on Raspberry Pi

This document summarizes the steps taken to install, configure, and fix
networking issues with Pi-hole running in Docker on a Raspberry Pi.

------------------------------------------------------------------------

## \## 1. Install & Run Pi-hole in Docker

We created a `docker-compose.yml` with Pi-hole running in **host
networking mode**, which is required for proper DNS behavior:

``` yaml
services:
  pihole:
    image: pihole/pihole:latest
    container_name: pihole
    network_mode: host
    environment:
      TZ: "Your_Timezone"
      WEBPASSWORD: "changeme"
    volumes:
      - ./etc-pihole:/etc/pihole
      - ./etc-dnsmasq.d:/etc/dnsmasq.d
    restart: unless-stopped
```

Run it using:

``` bash
docker compose up -d
```

------------------------------------------------------------------------

## 2. Access Pi-hole Web Interface

After deployment, Pi-hole is available here:

    http://<raspberry-pi-ip>/admin

------------------------------------------------------------------------

## 3. Reset or Set Admin Password

If no password was known, we reset it from inside the container:

``` bash
docker exec -it pihole pihole setpassword
```

Or set directly:

``` bash
docker exec -it pihole pihole setpassword 'newpassword'
```

------------------------------------------------------------------------

## 4. Router DNS Configuration

We attempted to point the router's DNS to Pi-hole, but the connection
broke due to:

    ignoring query from non-local network 192.168.1.1

This occurs when Pi-hole runs in the default Docker bridge network.

------------------------------------------------------------------------

## 5. Fix: Use Host Networking Mode

Switching Pi-hole to `network_mode: host` solved the issue.\
This allows Pi-hole to see real device IPs and accept LAN DNS traffic.

------------------------------------------------------------------------

## 6. Final Verification

After restarting the router and Raspberry Pi, DNS resolution worked
across the network.\
You can verify using:

    nslookup google.com

It should report the Raspberry Pi / Pi-hole IP as the DNS server.

------------------------------------------------------------------------

## ✓ Final Result

Pi-hole now works correctly:

-   Runs in Docker
-   Accepts LAN DNS requests
-   Filters entire network
-   Uses host networking for accurate IP handling
-   Login access restored

This document can be committed to GitHub as a reference for future
setups.

------------------------------------------------------------------------

## Author

Generated automatically by ChatGPT based on live debugging and
configuration steps.

Janos Rostas
