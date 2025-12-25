# Raspberry Pi Wi-Fi Connected but No Internet (Power Save Bug)

## Summary

This document describes a **common Raspberry Pi Wi-Fi failure mode** where the device:
- Appears connected to Wi-Fi
- Has a valid IP address and default route
- Cannot reach the router or the internet
- SSH and services become unreachable

The issue was observed on a Raspberry Pi running Docker containers and long uptimes.

---

## Symptoms

- `ip a` shows a valid IPv4 address on `wlan0`
- `ip route` shows a default route via the router
- `ping 8.8.8.8` fails (100% packet loss)
- `ping <router_ip>` returns:
  ```
  Destination host unreachable
  ```
- Restarting `wlan0` immediately restores connectivity

---

## Root Cause

The Raspberry Pi Wi-Fi driver enters a **broken power-save state**.

This results in:
- Successful association with the access point
- DHCP lease obtained
- Layer-2 traffic silently failing

This is a **known and recurring issue** on Raspberry Pi hardware, especially with:
- Long uptimes
- Power fluctuations
- Busy systems (Docker, Pi-hole, servers)
- 2.4 GHz networks

---

## Immediate Recovery (Manual Fix)

Reset the Wi-Fi interface:

```bash
sudo ip link set wlan0 down
sleep 5
sudo ip link set wlan0 up
```

Connectivity should be restored immediately.

---

## Permanent Mitigation (Recommended)

### 1. Disable Wi-Fi Power Saving Permanently

Create a NetworkManager configuration file:

```bash
sudo tee /etc/NetworkManager/conf.d/wifi-powersave.conf <<'EOF'
[connection]
wifi.powersave = 2
EOF
```

Restart NetworkManager:

```bash
sudo systemctl restart NetworkManager
```

Verify power saving is disabled:

```bash
iw dev wlan0 get power_save
```

Expected output:
```
Power save: off
```

---

### 2. Add a Wi-Fi Watchdog (Self-Healing)

Create a watchdog script:

```bash
sudo tee /usr/local/bin/wifi-watchdog.sh <<'EOF'
#!/bin/bash
ping -c1 -W2 192.168.1.1 >/dev/null || {
  logger "WiFi watchdog: resetting wlan0"
  ip link set wlan0 down
  sleep 5
  ip link set wlan0 up
}
EOF

sudo chmod +x /usr/local/bin/wifi-watchdog.sh
```

Add a cron job (runs every 5 minutes):

```bash
crontab -e
```

```cron
*/5 * * * * /usr/local/bin/wifi-watchdog.sh
```

---

## Why This Matters for Remote Systems

If the Raspberry Pi is:
- Headless
- Remote-only
- Located off-site

This issue can cause a **complete lockout** with no SSH access.

Disabling power save **reduces failures**, but the watchdog ensures **automatic recovery**, which is critical for remote reliability.

---

## Reliability Comparison

| Setup | Stability |
|------|----------|
| Default Wi-Fi | Days to weeks |
| Power save disabled | Weeks to months |
| Power save + watchdog | Effectively continuous |
| Ethernet | Years |

---

## Recommendations

- Prefer **Ethernet** for critical systems
- Disable Wi-Fi power saving
- Always use a watchdog or recovery mechanism
- Consider reverse SSH tunnels for remote recovery

---

## Keywords

Raspberry Pi, Wi-Fi, power save, NetworkManager, wlan0, Docker, Pi-hole, headless, remote access, SSH timeout
